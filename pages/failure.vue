<template>
    <div class="page">
        <h1>Ошибка при выполнении транзакции</h1>
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
    }
    ,
    methods: {
        sendRequest(reference) {
            const token = this.getAuthorizationCookie();
            const path = `${this.pathUrl}/api/money/failure/${reference}`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    if (response.status == 200) {
                        //  window.location.href = '/'
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        },
    },
    mounted() {
        const url = window.location.href;
        const match = url.match(/order_pay_tf2shop_(\d+)/);

        if (match) {
            this.extractedValue = match[0];
            console.log(this.extractedValue);

            this.sendRequest(match[0])
        }
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Ошибка при выполнении транзакции | DotaShop',
    ogTitle: 'Ошибка при выполнении транзакции | DotaShop',
    description: 'Ошибка при выполнении транзакции | DotaShop',
    ogDescription: 'Ошибка при выполнении транзакции | DotaShop',
})
</script>
<style lang="scss" scoped>
.page {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

h1 {
    font-size: 42px;
    font-family: var(--col);
    color: #fff;
    text-align: center;

    @media (max-width: 1024px) {
        font-size: 25px;
    }
}
</style>