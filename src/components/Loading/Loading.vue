<template>
    <img
        src="../../assets/img/loading.png"
        :style="{ display: showing ? 'flex' : 'none' }"
        alt=""
        class="loading"
    />
</template>

<script>
export default {
    name: "Loading",
    props: ["showing"],
};
</script>

<style>
@keyframes loading-spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
.loading {
    animation: loading-spin 1s linear infinite;

    position: fixed;
    left: 20px;
    top: 20px;
}
</style>
