<template>
    <div
        id="card"
        class="card"
        :style="{
            background: `linear-gradient(0deg, rgb(0, 0, 0,0.70) 10%, rgba(0, 0, 0, 0) 50%),url(${img})`,
            'background-size': 'cover',
            'background-position': 'center',
        }"
    >
        <p class="card__name">{{ name }}</p>
    </div>
</template>

<script>
export default {
    props: {
        img: {
            type: String,
            default: "",
            required: true,
        },
        name: {
            type: String,
            default: "",
            required: true,
        },
    },
};
</script>

<style scoped lang="css">
.card {
    margin-left: 20px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-end;
    padding: 10px;
    width: 22vw;
    border-radius: 12px;
    height: 25vh;
    color: white;
    font-size: 2.25rem;
}

.card:hover {
    box-shadow: 0px 0px 10px rgb(0, 0, 0);
}
</style>
