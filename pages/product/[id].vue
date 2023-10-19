<template>
    <div class="page">
        <prevPage></prevPage>
        <div v-if="product.length <= 0"></div>
        <div class="product" v-else>
            <div class="product__price">
                <div class="product__image">
                    <img :src="product.img" alt="">
                </div>

                <div class="text-center price">
                    <h2>{{ product.sell_price.toFixed(1).toLocaleString() }} ₸</h2>
                    <button ref="cart" @click="addCart">Купить</button>
                </div>
            </div>

            <div class="product__desc">
                <h1>{{ product.name }}</h1>

                <div class="product__body">
                    <div class="tags">
                        <span>Класс: {{ product.tags['Класс'].name }}</span>
                        <span>Качество: {{ product.tags['Качество'].name }}</span>
                        <span>Тип: {{ product.tags['Тип'].name }}</span>
                    </div>
                    <div class="description" v-html="product.descriptions"></div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            productId: this.$route.params.id,
            product: [],
            description: 'Жар решимости Дэвиона ненадолго сливает воедино элементы и материю',
            pathUrl: 'https://tf2shop.kz',
        }
    },
    methods: {
        addCart() {
            const url = `${this.pathUrl}/api/products/add-busket-item/${this.productId}`
            const token = this.getAuthorizationCookie();

            this.$refs.cart.innerHTML = 'Добавляем'
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(url)
                .then((res) => {
                    console.log(res)

                    if (res.status == 201) {
                        this.$refs.cart.innerHTML = 'Добавлен'
                    }
                    else (
                        this.$refs.cart.innerHTML = 'Ошибка'
                    )
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        getProduct() {
            const url = `${this.pathUrl}/api/products/get-detail-item/${this.productId}`
            axios
                .get(url)
                .then(response => {
                    this.product = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getProduct()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Страница товара | TF2Shop',
    ogTitle: 'Страница товара | TF2Shop',
    description: 'Страница товара | TF2Shop',
    ogDescription: 'Страница товара | TF2Shop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 130px 70px 40px;

    @media (max-width: 1600px) {
        padding: 130px 50px 40px;
    }

    @media (max-width: 1024px) {
        padding: 130px 20px 40px;
    }

    .product {
        margin-top: 30px;
        display: flex;
        align-items: flex-start;
        gap: 30px;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
            justify-content: center;
            align-items: center;
        }

        .product__desc {

            .product__body {
                display: flex;
                gap: 5.208vw;

                @media (max-width: 1024px) {
                    flex-direction: column;
                    gap: 20px;
                }

                .tags {
                    display: flex;
                    flex-direction: column;
                    gap: 10px;
                }

                .description {
                    max-width: 900px;
                }

                span,
                div {
                    display: block;
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: 130%;
                    font-family: var(--geo);
                    color: #fff;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }

            h1 {
                font-size: 36px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;
                margin: 0 0 20px;

                @media (max-width: 1024px) {
                    font-size: 20px;
                }
            }
        }

        .product__price {

            .price {
                margin-top: 20px;
                display: flex;
                flex-direction: column;
                gap: 20px;
                align-items: center;
                justify-content: center;

                h2 {
                    margin: 0;
                    font-size: 32px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: 130%;
                    font-family: var(--geo);
                    color: #fff;
                }

                button {
                    border-radius: 10px;
                    background: #FF1D00;
                    padding: 13px 15px;
                    border: 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--geo);
                    width: 100%;
                    max-width: 80%;
                    color: #fff;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }

            .product__image {
                border-radius: 10px;
                background: linear-gradient(180deg, #7C3B3B 0%, #030239 100%);
                padding: 5px 10px;

                @media (max-width: 1024px) {
                    width: 100%;
                }


                img {
                    width: 100%;
                    height: 197px;
                    object-fit: cover;

                    @media (max-width: 1024px) {
                        height: 247px;
                    }
                }
            }
        }
    }
}
</style>