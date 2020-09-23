<template>
    <div class="post">
        <Autor :postInfo="postInfo" />
        <div
            :style="{ backgroundImage: `url(${postInfo.urls.regular})` }"
            class="post__picture-container"
            :title="postInfo.description"
        ></div>
        <Views :postId="postInfo.id" :token="token" :error="error" />
    </div>
</template>

<script>
import Autor from "./Autor";
import Views from "./Views";

export default {
    name: "Post",
    props: ["token", "error", "postInfo"],

    components: {
        Autor,
        Views,
    },
};
</script>

<style>
.post {
    min-width: 320px;
    margin-left: 20px;
}

.post:nth-child(odd) {
    margin-left: 0px;
}

.post__picture-container {
    width: 100%;
    padding-bottom: 71.875%;

    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
}

@media screen and (max-width: 999px) {
    .post {
        max-width: calc(50% - 10px);
    }
}

@media screen and (max-width: 767px) {
    .post {
        min-width: 100%;
        margin: 0;
    }
}
</style>
