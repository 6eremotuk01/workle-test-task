<template>
    <a :href="postInfo.user.links.html" target="_blank">
        <div class="post__autor">
            <img
                v-bind:src="postInfo.user.profile_image.small"
                alt=""
                class="post__autor__picture"
            />
            <div class="post__autor__text">
                <h5 class="autor__text__name">
                    {{ postInfo.user.name }}
                </h5>
                <p class="autor__text__link">@{{ postInfo.user.username }}</p>
            </div>
        </div>
    </a>
</template>

<script>
export default {
    name: "Autor",
    props: ["postInfo"],
    components: {},
};
</script>

<style scope>
a {
    text-decoration: none;
}
.post__autor {
    display: flex;
    margin: 10px;
}

.post__autor__picture {
    display: block;

    width: 30px;
    height: 30px;

    border-radius: 100%;

    background: gray;
}

.post__autor__text {
    margin-left: 10px;
}

.autor__text__name {
    font-family: "Roboto Condensed";
    font-style: normal;
    font-weight: bold;
    font-size: 12px;
    line-height: 14px;

    color: #333333;

    margin: 0px;
    padding: 0px;
}

.autor__text__link {
    font-family: "Roboto Condensed";
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 14px;

    color: #8d8d8d;

    margin: 0px;
    padding: 0px;

    text-decoration: none;
}

@media screen and (max-width: 767px) {
    .post__autor {
        margin-left: 0px;
        margin-right: 0px;
    }
}

@media screen and (max-width: 479px) {
    .post__autor {
        margin: 10px;
    }
}
</style>
