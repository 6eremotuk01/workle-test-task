<template>
    <div class="pagenation">
        <div class="pagenation__pages-wrapper">
            <p
                v-if="!(+pages.current === 1 || +pages.current === 2)"
                class="pagenation__first-page"
                @click="goToPage(1)"
            >
                1
            </p>
            <p
                v-if="previousPage"
                class="pagenation__previous-page"
                @click="goToPage(previousPage)"
            >
                {{ previousPage }}
            </p>
            <p
                v-if="previousBeforePreviousPage"
                class="pagenation__previous-page"
                @click="goToPage(previousBeforePreviousPage)"
            >
                {{ format(previousBeforePreviousPage) }}
            </p>
            <p class="pagenation__current-page">{{ currentPage }}</p>
            <p
                v-if="nextPage"
                class="pagenation__next-page"
                @click="goToPage(nextPage)"
            >
                {{ nextPage }}
            </p>
            <p
                v-if="nextAfterNextPage"
                class="pagenation__next-page"
                @click="goToPage(nextAfterNextPage)"
            >
                {{ nextAfterNextPage }}
            </p>
            <p
                v-if="pages.current !== pages.total"
                class="pagenation__last-page"
                @click="goToPage(pages.total)"
            >
                {{ pages.total }}
            </p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Pagenation",

    props: ["pages", "getPageContentByNumber"],

    methods: {
        format(digit) {
            return digit
                .toString()
                .split(/(?=(?:\d{3})+(?!\d))/g)
                .join(" ");
        },

        goToPage(pageNumber) {
            window.scroll(0, 0);
            this.getPageContentByNumber(pageNumber);
            console.log(this.pages.total);
        },
    },

    computed: {
        currentPage() {
            return +this.pages.current;
        },

        // Номер следующей страницы
        nextPage() {
            let nextPage = +this.pages.current + 1;
            return nextPage <= this.pages.total ? nextPage : null;
        },

        // Номер предыдущей страницы
        previousPage() {
            let previousPage = +this.pages.current - 1;
            return previousPage >= 1 ? previousPage : null;
        },

        // Номер следующей страницы после следующей
        nextAfterNextPage() {
            let nextAfterNextPage = +this.pages.current + 2;
            return this.pages.current === 1 ? nextAfterNextPage : null;
        },

        // Номер предыдущей страницы перед предыдущей
        previousBeforePreviousPage() {
            let previousBeforePreviousPage = +this.pages.current - 2;
            return this.pages.current === this.pages.total
                ? previousBeforePreviousPage
                : null;
        },
    },
};
</script>

<style>
.pagenation {
    display: flex;
    justify-content: center;
    align-items: center;

    position: fixed;
    bottom: -100px;
    left: 0px;

    height: 160px;
    width: 100%;

    background: rgba(0, 0, 0, 0.9);
    color: white;

    font-family: "Roboto Condensed";
    font-style: normal;
    font-weight: normal;
    font-size: 14px;
    line-height: 60px;

    padding-bottom: 100px;
}

.pagenation__pages-wrapper {
    display: flex;
    justify-content: center;
    align-items: baseline;
}

.pagenation__first-page {
    padding-right: 10px;
    cursor: pointer;
}

.pagenation__first-page::after {
    content: "...";
    display: inline;
    padding-left: 10px;
}

.pagenation__previous-page {
    padding-right: 10px;
    cursor: pointer;
}

.pagenation__current-page {
    padding-right: 10px;
    font-size: 18px;
    cursor: pointer;
    font-weight: bold;
}

.pagenation__next-page {
    padding-right: 10px;
    cursor: pointer;
}

.pagenation__last-page {
    cursor: pointer;
}

.pagenation__last-page::before {
    content: "...";
    display: inline;
    padding-right: 10px;
}

.pagenation a {
    color: white;
}
</style>
