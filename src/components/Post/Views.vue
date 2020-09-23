<template>
    <div class="post__views">
        <p class="post__views__count">{{ formattedViews }}</p>
        <div class="post__views__eye" />
    </div>
</template>

<script>
export default {
    name: "Views",

    props: ["postId", "token"],

    data() {
        return {
            totalViews: "∞",
            error: {
                status: false,
                header: "",
                text: "",
            },
        };
    },

    methods: {
        // Загрузка информации о количестве просмотров поста
        loadViewsInfo() {
            try {
                fetch(
                    `https://api.unsplash.com/photos/${this.postId}/statistics/?client_id=${this.token}`
                )
                    .then((response) => {
                        return response.json();
                    })
                    .then((json) => {
                        this.totalViews = json.views.total.toString();
                    });
            } catch (errorProperties) {
                console.log(errorProperties);
                this.error = {
                    status: true,
                    header: "Ошибка",
                    text: "Более подробная информация отображена в консоли",
                };
            }
        },
    },

    computed: {
        // Форматируем вывод счетчика
        formattedViews() {
            return this.totalViews.split(/(?=(?:\d{3})+(?!\d))/g).join(" ");
        },
    },

    mounted() {
        this.loadViewsInfo();
    },
};
</script>

<style>
.post__views {
    display: flex;
    justify-content: flex-end;
    align-items: center;

    margin: 10px;
}

.post__views__eye {
    width: 18px;
    height: 12px;

    background: url("../../assets/img/eye.png");
    background-position: center center;
}

.post__views__count {
    margin: 0;
    margin-right: 5px;

    padding: 0;

    height: 12px;

    font-family: "Roboto Condensed";
    font-style: normal;
    font-weight: bold;
    font-size: 12px;
    line-height: 14px;

    text-align: right;

    color: #8d8d8d;
}

@media screen and (max-width: 767px) {
    .post__views {
        margin-left: 0px;
        margin-right: 0px;
    }
}

@media screen and (max-width: 479px) {
    .post__views {
        margin: 10px;
    }
}
</style>
