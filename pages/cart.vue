<template>
    <div class="page">
        <h1>Подтверждение покупки</h1>

        <div v-if="cart.length <= 0"></div>
        <div class="confirm" v-else>
            <div class="catalog">
                <div class="catalog__items">
                    <NuxtLink v-for="item in cart" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
                        <small>-{{ Math.floor(100 - (item.item.sell_price / item.item.price * 100)) }}%</small>
                        <img :src="item.item.img" alt="" class="item__image">
                        <p>{{ item.item.name }}</p>
                        <div class="price">
                            <small>{{ item.item.price.toFixed(1).toLocaleString() }} ₸</small>
                            <span>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                        </div>
                    </NuxtLink>

                </div>
            </div>
            <div class="cart__info">
                <span>Количество товаров: {{ cart.length }}</span>
                <span>Итого: {{ totalAmount }} ₸ </span>
                <button ref="purchases" @click="confirmPur">Подтвердить покупку</button>
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
            pathUrl: 'https://tf2shop.kz',
            cart: [],
        }
    },
    computed: {
        totalAmount() {
            return this.cart.reduce((total, item) => {
                return total + item.item.sell_price;
            }, 0).toFixed(1).toLocaleString();
        },
    },

    methods: {
        confirmPur() {
            const url = `${this.pathUrl}/api/products/confirm-busket`
            this.$refs.purchases.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchases.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error.message);
                    if (error.message == 'Request failed with status code 304') {
                        this.$refs.purchases.innerHTML = 'Недостаточно средств'
                    }
                });
        },
        deleteItem(id) {
            const url = `${this.pathUrl}/api/products/delete-busket-item/${id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.getCart()
                })
        },
        getCart() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    this.balance = res.data.balance
                    this.cart = res.data.basket
                })
        },
    },
    mounted() {
        this.getCart()
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Подтверждение покупки | TF2Shop',
    ogTitle: 'Подтверждение покупки | TF2Shop',
    description: 'Подтверждение покупки | TF2Shop',
    ogDescription: 'Подтверждение покупки | TF2Shop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 130px 163px 40px 70px;

    @media (max-width: 1600px) {
        padding: 130px 50px 40px;
    }

    @media (max-width: 1024px) {
        padding: 130px 20px 40px;
    }

    .confirm {
        display: flex;
        align-items: flex-start;
        gap: 3.802vw;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 20px;
        }

        .cart__info {
            border-radius: 10px;
            border: 2px solid #FF1D00;
            background: #2A1310;
            padding: 20px 20px 37px;
            min-width: 22.656vw;
            display: flex;
            flex-direction: column;
            gap: 20px;
            justify-content: center;
            align-items: center;

            @media (max-width: 1300px) {
                min-width: 400px;
            }

            @media (max-width: 1024px) {
                min-width: 100%;
                align-items: flex-start;
                padding: 20px;
            }

            span {
                display: block;
                font-size: 24px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1600px) {
                    font-size: 20px;
                }
            }

            button {
                border-radius: 10px;
                background: #0500FF;
                border: 0;
                padding: 10px 20px;
                margin-top: 10px;

                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }
        }

        .catalog {
            width: 100%;

            .catalog__items {
                display: grid;
                grid-template-columns: repeat(auto-fill, minmax(279px, 1fr));
                gap: 21px;
                grid-auto-flow: dense;

                @media (max-width: 1024px) {
                    gap: 20px;
                    grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
                }

                .catalog__item {
                    padding: 5px 10px;
                    border-radius: 10px;
                    position: relative;
                    text-decoration: none;
                    background: linear-gradient(180deg, #7C3B3B 0%, #030239 100%);

                    display: flex;
                    flex-direction: column;

                    .item__image {
                        width: 100%;
                        height: 197px;
                        object-fit: cover;
                        margin: 0 0 10px;

                        @media (max-width: 1024px) {
                            height: 110px;
                        }
                    }

                    small {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        position: absolute;
                    }

                    p {
                        margin: 0 0 5px;
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 300;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 12px;
                        }
                    }

                    .price {
                        margin-top: auto;
                        display: flex;
                        gap: 5px;
                        justify-content: flex-end;

                        small {
                            font-size: 12px;
                            font-style: normal;
                            font-weight: 300;
                            line-height: 130%;
                            font-family: var(--geo);
                            color: #fff;
                            text-decoration: line-through;
                            position: relative;
                        }
                    }

                    span {
                        text-align: right;
                        font-size: 16.424px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        margin-top: auto;
                    }
                }
            }
        }
    }

    h1 {
        font-size: 36px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%;
        font-family: var(--fun);
        color: #fff;
        margin: 0 0 30px;
    }
}
</style>