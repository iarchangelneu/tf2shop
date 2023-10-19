<template>
    <div class="back">
        <button @click="goBack">
            <img src="@/assets/img/back.svg" alt="" loading="lazy">
            <span>Назад</span>
        </button>
    </div>
</template>
<script>
export default {
    methods: {
        goBack() {
            this.prevPage = this.$nuxt.$router.go(-1);
        },
    },
}
</script>
<style lang="scss" scoped>
.back {
    button {
        border-radius: 10px;
        background: #0500FF;
        border: 0;
        padding: 10px 20px;
        display: flex;
        align-items: center;
        gap: 10px;

        font-size: 16px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        font-family: var(--geo);
        color: #fff;
    }
}
</style>