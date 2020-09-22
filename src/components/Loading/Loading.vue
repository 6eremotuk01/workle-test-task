<template>
    <img
        :style="{ opacity: 1 * showing }"
        src="../../assets/loading.png"
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
    left: 10px;
    top: 10px;
}
</style>
