<template>
    <div class="error">
        <h1 class="error__header">{{ error.header }}</h1>
        <p class="error__text">{{ error.text }}</p>
        <div class="error__button-wrapper">
            <button class="error__repeat-button" @click="repeatMethod">
                🔁 Повторить
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: "Error",
    props: ["error", "repeatMethod"],
};
</script>

<style>
.error {
    font-family: "Roboto Condensed";

    width: 660px;
    margin: 20px auto;

    padding: 20px;

    background: #ffd2cf;
}

.error__header {
    margin: 0px;
}

.error__text {
    margin: 0px;
    margin-top: 10px;
}

.error__button-wrapper {
    margin-top: 10px;
    text-align: right;
}

.error__repeat-button {
    font-family: "Roboto Condensed";

    background: white;
    border: black 1px solid;
    padding: 5px 10px;
    border-radius: 3px;
    cursor: pointer;
}
</style>
