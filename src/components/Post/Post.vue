<template>
    <div class="post">
        <Autor
            :autorName="autorName"
            :autorId="autorId"
            :autorPicture="autorPicture"
            :profileUrl="profileUrl"
        />

        <div class="post__picture-wrapper" :title="pictureAlt">
            <img :src="postPicture" :alt="pictureAlt" class="post__picture" />
        </div>

        <Views :postId="postId" :token="token" :error="error" />
    </div>
</template>

<script>
import Autor from "./Autor";
import Views from "./Views";

export default {
    name: "Post",
    props: [
        "autorName",
        "autorId",
        "postPicture",
        "autorPicture",
        "profileUrl",
        "pictureAlt",
        "postId",
        "token",
        "error",
    ],

    components: {
        Autor,
        Views,
    },
};
</script>

<style>
.post {
    max-width: 320px;
}

.post__picture-wrapper {
    width: 100%;
}

.post__picture {
    max-width: 100%;
}

@media screen and (max-width: 999px) {
    .post {
        max-width: calc(50% - 10px);
    }
}

@media screen and (max-width: 767px) {
    .post {
        max-width: 100%;
    }
}
</style>
