<template>
    <div id="app">
        <Loading v-if="showLoadingWheel" :showing="loading" />
        <Error
            v-if="error.status && showUserErrorMessage"
            :error="error"
            :repeatMethod="getPageContentByNumber.bind(this, 1)"
        />
        <div class="posts" v-if="!error.status">
            <Post
                v-for="(post, index) in posts.current"
                v-bind:key="index"
                :postPicture="post.urls.regular"
                :autorPicture="post.user.profile_image.small"
                :autorName="post.user.name"
                :autorId="post.user.username"
                :profileUrl="post.user.links.html"
                :pictureAlt="post.description"
                :postId="post.id"
                :token="query.token"
            />
        </div>
        <Pagenation
            v-if="!error.status && !loading"
            :pages="pages"
            :getPageContentByNumber="getPageContentByNumber"
        />
    </div>
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
            showLoadingWheel: true, // Отображение колёсика загрузки
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
                searchQuery: "minimal",
                perPage: 20,
                type: "https://api.unsplash.com/search/photos/",
                string: undefined,
            },

            // Настройка запроса
            error: {
                status: false,
                text: "",
            },
        };
    },

    methods: {
        // Получаем посты, на указанной странице
        //      pageNumber — новый номер страницы
        getPageContentByNumber(pageNumber) {
            // Необходим для предотвращения загрузки следующей страницы, при быстрых нажатиях пользователя
            if (this.loading) {
                return;
            }

            this.error = {
                status: false,
                header: "",
                text: "",
            };
            this.loading = true;

            //Формируем новый запрос
            this.pages.current = pageNumber;
            this.query.string = `${this.query.type}?client_id=${this.query.token}&query=${this.query.searchQuery}&page=${this.pages.current}&perpage=${this.query.perPage}`;

            try {
                // Отправка сформированного запроса
                fetch(this.query.string)
                    .then((response) => {
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
                    })

                    .then(
                        // Запрос произошёл удачно
                        (jsonData) => {
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
                                    text: `Сервер ответил ошибкой. Текст ошибки ${jsonData.errors.join(
                                        "\n"
                                    )}`,
                                };
                            }

                            // Записываем данные
                            this.posts = {
                                current: jsonData.results,
                                total: jsonData.total,
                            };
                            this.pages = {
                                current: pageNumber,
                                total: jsonData.total_pages,
                            };

                            // Завершаем з
                            this.loading = false;
                        },

                        // Запрос прошёл с неудачей
                        (error) => {
                            this.loading = false;
                            throw {
                                status: true,
                                header: "Ошибка",
                                text: `Сервер ответил ошибкой. Текст ошибки ${error.toString()}`,
                            };
                        }
                    )
                    .catch((errorProperties) => {
                        if (errorProperties.header) {
                            this.error = errorProperties;
                        } else {
                            this.error = {
                                status: true,
                                header: "Ошибка",
                                text: errorProperties,
                            };
                        }

                        if (!this.showUserErrorMessage)
                            console.log(this.error.text);

                        this.loading = false;
                    });
            } catch (errorProperties) {
                // Обрабатываем все ошибки
                if (errorProperties.header) {
                    this.error = errorProperties;
                } else {
                    this.error = {
                        status: true,
                        header: "Ошибка",
                        text: errorProperties,
                    };
                }

                if (!this.showUserErrorMessage) console.log(this.error.text);

                this.loading = false;
            }
        },

        isEmptyObject(object) {
            for (let key in object) {
                return false;
            }
            return true;
        },
    },

    mounted() {
        // Берем результаты с сервера
        this.getPageContentByNumber(this.pages.current);
    },
};
</script>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
}

.posts {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin: 0 auto;
    width: 660px;
    padding-bottom: 70px;
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
