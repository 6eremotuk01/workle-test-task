<template>
    <div
        :style="{ display: showing ? 'flex' : 'none' }"
        class="loading-wrapper"
    >
        <img src="../../assets/loading.png" alt="" class="loading" />
    </div>
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
}

.loading-wrapper {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(0, 0, 0, 0.05);
}
</style>
