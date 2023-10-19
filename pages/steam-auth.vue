<template>
    <div class="page">
        <h1>Авторизация</h1>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            pathUrl: 'https://tf2shop.kz',
        }
    },
    methods: {
        steamLogin() {
            let urlParams = new URLSearchParams(window.location.search);
            let token = urlParams.get('token')
            localStorage.setItem('accountType', 'steam')
            document.cookie = `Authorization=${token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;

            this.$router.push('/account')
        }
    },
    mounted() {
        this.steamLogin()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Авторизация | DotaShop',
    ogTitle: 'Авторизация | DotaShop',
    description: 'Авторизация | DotaShop',
    ogDescription: 'Авторизация | DotaShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 130px 70px 40px;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    h1 {
        font-family: var(--fun);
        font-size: 42px;
        color: #fff;
        text-align: center;
    }
}
</style>