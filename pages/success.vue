<template>
    <div class="page">
        <h1>Транзакция прошла успешно!</h1>
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
            const path = `${this.pathUrl}/api/money/success/${reference}`;
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
    title: 'Успешная транзакция | DotaShop',
    ogTitle: 'Успешная транзакция | DotaShop',
    description: 'Успешная транзакция | DotaShop',
    ogDescription: 'Успешная транзакция | DotaShop',
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