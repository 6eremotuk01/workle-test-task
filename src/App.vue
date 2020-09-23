<template>
    <main id="app">
        <Loading v-if="showLoadingWheel" :showing="loading" />
        <Error
            v-if="error.status && showUserErrorMessage"
            :error="error"
            :repeatMethod="getPageContentByNumber.bind(this, 1)"
        />

        <div v-if="!error.status">
            <div class="posts">
                <Post
                    v-for="(post, index) in posts.current"
                    :key="index"
                    :token="query.token"
                    :postInfo="post"
                />
            </div>
            <Pagenation
                :pages="pages"
                :getPageContentByNumber="getPageContentByNumber"
            />
        </div>
    </main>
</template>

<script>
import Post from "./components/Post/Post";
import Pagenation from "./components/Pagenation/Pagenation";
import Error from "./components/Error/Error";
import Loading from "./components/Loading/Loading";

export default {
    name: "App",

    components: {
        Post,
        Pagenation,
        Error,
        Loading,
    },

    data: () => {
        return {
            loading: false,

            showLoadingWheel: false, // Отображение колёсика загрузки
            showUserErrorMessage: true, // Отображение информации об ошибке пользователю

            // Информация о постах
            posts: {
                current: [],
                total: undefined,
            },

            // Информация о страницах
            pages: {
                current: 1,
                total: undefined,
            },

            // Настройка запроса
            query: {
                token: "mOQX6Cj3-f4YVuy4XM3su5MGBUOW5HXPF_XDyEFeWPs",
                searchQuery: "minimalism",
                perPage: 20,
                type: "https://api.unsplash.com/search/photos/",
                string: undefined,
            },

            // Настройка запроса
            error: {
                status: false,
                herader: undefined,
                text: undefined,
            },
        };
    },

    methods: {
        // Обработка ошибок запроса
        checkResponse(response) {
            // Если страница не найдена
            if (response.status === 404) {
                throw {
                    status: true,
                    header: "Ошибка 404",
                    text: "Адрес не найден.",
                };
            }

            // Если у сервера произошла внутренняя ошибка
            if (response.status === 500) {
                throw {
                    status: true,
                    header: "Ошибка 500",
                    text: "Внутренняя ошибка сервера.",
                };
            }

            // Возвращаем результат
            return response.json();
        },

        // Обработка успешного запроса
        //      jsonData — результат запроса
        successfulResults(jsonData) {
            // Если запрос веррнул пустые данные
            if (this.isEmptyObject(jsonData)) {
                throw {
                    status: true,
                    header: "Ошибка",
                    text:
                        "Запрос к unsplash.com не ответил результатом. Возможно, что сервис не работает на данный момент. \n\nИли ваш лимит запросов был исчерпан.",
                };
            }

            // Если на запрос ответили ошибкой
            if (jsonData.errors) {
                throw {
                    status: true,
                    header: "Ошибка",
                    text: `Сервер ответил ошибкой:\n${jsonData.errors.join(
                        "\n"
                    )}`,
                };
            }

            // Записываем данные
            this.posts = {
                current: jsonData.results,
                total: jsonData.total,
            };
            this.pages.total = jsonData.total_pages;

            // Завершаем з
            this.loading = false;
        },

        // Обработка неудачного запроса
        //      error — данные запроса с ошибкой
        unsuccessfulResult(error) {
            this.loading = false;
            throw {
                status: true,
                header: "Ошибка",
                text: `Сервер ответил ошибкой:\n${error.toString()}`,
            };
        },

        // Обработка пойманных ошибок
        //      catchedError — пойманая ошибка блоком catch
        errorProcessing(catchedError) {
            if (catchedError.header) {
                this.error = catchedError;
            } else {
                this.error = {
                    status: true,
                    header: "Ошибка",
                    text: catchedError,
                };
            }

            if (!this.showUserErrorMessage) console.log(this.error.text);

            this.loading = false;
        },

        // Проверка объекта на пустоту
        //      object — проверяемый объект
        isEmptyObject(object) {
            for (let key in object) {
                return false;
            }
            return true;
        },

        // Получаем посты, на указанной странице
        //      pageNumber — новый номер страницы
        getPageContentByNumber(pageNumber) {
            // Необходим для предотвращения загрузки следующей страницы, при быстрых нажатиях пользователя
            if (this.loading) {
                return;
            }

            this.loading = true;

            this.error = {
                status: false,
                header: "",
                text: "",
            };

            //Формируем новый запрос
            this.pages.current = pageNumber;
            this.query.string = `${this.query.type}?client_id=${this.query.token}&query=${this.query.searchQuery}&page=${this.pages.current}&perpage=${this.query.perPage}`;

            try {
                // Отправка сформированного запроса
                fetch(this.query.string)
                    .then((response) => this.checkResponse(response))
                    .then(
                        // Запрос произошёл удачно
                        (jsonData) => this.successfulResults(jsonData),

                        // Запрос прошёл с неудачей
                        (error) => this.unsuccessfulResult(error)
                    )
                    .catch((catchedError) =>
                        this.errorProcessing(catchedError)
                    );
            } catch (catchedError) {
                this.errorProcessing(catchedError);
            }
        },
    },

    mounted() {
        // Берем результаты с сервера
        this.getPageContentByNumber(this.pages.current);
    },
};
</script>

<style>
@import url("./assets/fonts/fonts.css");

* {
    box-sizing: border-box;
}

body {
    margin: 0;
}

.posts {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    margin: 0 auto;

    padding-bottom: 70px;

    width: 660px;
}

@media screen and (max-width: 999px) {
    .posts {
        width: 100%;

        padding-left: 54px;
        padding-right: 54px;
    }
}

@media screen and (max-width: 767px) {
    .posts {
        flex-direction: column;

        padding-left: 20px;
        padding-right: 20px;
    }
}

@media screen and (max-width: 479px) {
    .posts {
        flex-direction: column;

        padding-left: 0px;
        padding-right: 0px;
    }
}
</style>
