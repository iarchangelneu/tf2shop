<template>
    <header v-if="!hideHeaderOnPages.includes($route.name)">
        <div class="header__mob">
            <img src="@/assets/img/headerlogomob.svg" alt="">

            <div class="user">
                <img src="@/assets/img/cart.svg" style="cursor: pointer;" @click.stop="toggleCart" alt="">

                <div class="burg">
                    <input id="menu__toggle" class="d-none" type="checkbox" />
                    <label class="menu__btn" for="menu__toggle" @click="menuOpen = !menuOpen">
                        <span></span>
                    </label>
                </div>
            </div>

            <div class="mobmenu" :class="{ activeMenu: menuOpen }">
                <div class="menushka text-center">
                    <div class="links">
                        <NuxtLink to="/sale" @click="menuOpen = !menuOpen">Продажа</NuxtLink>
                        <NuxtLink to="/catalog" @click="menuOpen = !menuOpen">Магазин</NuxtLink>
                        <a href="/#about" @click="menuOpen = !menuOpen">О нас</a>
                        <a href="/#faq" @click="menuOpen = !menuOpen">FAQ</a>
                    </div>

                    <NuxtLink to="/withdrawal" class="cash" v-if="isAuth">
                        <img src="@/assets/img/cash.svg" alt="">
                        <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                            balance.toFixed(1).toLocaleString()
                            + ' ₸' }}</span>
                    </NuxtLink>

                    <NuxtLink to="/login" v-if="!isAuth">Вход / регистрация</NuxtLink>
                    <NuxtLink to="/account" v-else>Личный кабинет</NuxtLink>
                </div>
            </div>
        </div>
        <div class="header__pc">
            <NuxtLink to="/">
                <img src="@/assets/img/headerlogo.svg" class="img-fluid" alt="">
            </NuxtLink>

            <div class="links">
                <NuxtLink to="/sale">Продажа</NuxtLink>
                <NuxtLink to="/catalog">Магазин</NuxtLink>
                <a href="/#about">О нас</a>
                <a href="/#faq">FAQ</a>
            </div>

            <div class="user">
                <img src="@/assets/img/cart.svg" v-if="isAuth" style="cursor: pointer;" @click.stop="toggleCart" alt="">
                <NuxtLink to="/withdrawal" class="cash" v-if="isAuth">
                    <img src="@/assets/img/cash.svg" alt="">
                    <span v-if="balance !== null">{{ balance == null ? '0 ₸' :
                        balance.toFixed(1).toLocaleString()
                        + ' ₸' }}</span>
                </NuxtLink>
                <NuxtLink to="/login" v-if="!isAuth">Вход / регистрация</NuxtLink>
                <NuxtLink to="/account" v-else>Личный кабинет</NuxtLink>
            </div>
        </div>
    </header>
    <div class="cart" :class="{ cartOpen: cartOpen }" @click="stopPropagation">
        <div class="d-flex justify-content-end">
            <span class="closespan text-right" @click.stop="toggleCart">Закрыть</span>
        </div>

        <div class="cart__body">
            <div class="cart__item" v-for="item in cart" :key="item.id">
                <div class="img">
                    <img :src="item.item.img" alt="">
                </div>

                <div class="w-100">
                    <div class="text-right">
                        <img src="@/assets/img/close.svg" style="cursor: pointer;" @click="deleteItem(item.id)">
                    </div>
                    <div class="delete">
                        <h2>{{ item.item.name }}</h2>
                    </div>

                    <span>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                </div>
            </div>
        </div>

        <div class="cart__footer mt-4">
            <p>иТОГО: {{ totalAmount }} ₸ </p>

            <div class="btns">
                <NuxtLink to="/cart">
                    Оформить заказ
                </NuxtLink>
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
            sideActive: false,
            cartOpen: false,
            hideHeaderOnPages: ['login', 'register'],
            menuOpen: false,
            pathUrl: 'https://tf2shop.kz',
            cart: [],
            balance: null,
            isAuth: false,
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
        calculateTotal() {
            let total = 0;

            this.cart.forEach(item => {
                const price = item.products;
                const discountedPrice = price;
                total += discountedPrice * item.amount;
            });

            return total;
        },
        confirmPur() {
            const url = `${this.pathUrl}/api/products/confirm-busket`
            this.$refs.purchase.innerHTML = 'Покупаем'

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getCart()
                    this.$refs.purchase.innerHTML = 'Куплено'
                })
                .catch(error => {
                    console.error(error);
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
        toggleCart() {
            this.cartOpen = !this.cartOpen;
            if (this.cartOpen) {
                document.addEventListener('click', this.closeCart);
                this.getCart()
            } else {
                document.removeEventListener('click', this.closeCart);
            }
        },
        closeCart() {
            this.cartOpen = false;
            document.removeEventListener('click', this.closeCart);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getCart()
            this.isAuth = true
        }
        else {
            this.isAuth = false
        }

    }
}
</script>
<style lang="scss" scoped>
header {
    position: fixed;
    width: 100%;
    z-index: 200;

    .header__mob {
        display: none;

        @media(max-width: 1024px) {
            display: flex;
            justify-content: space-between;
            padding: 60px 20px 30px;
            background: #000;
            position: relative;
        }

        .user {
            display: flex;
            gap: 30px;
            align-items: center;
        }

        .activeMenu {
            right: 0 !important;
        }

        .mobmenu {
            position: fixed;
            width: 100vw;
            right: -3000px;
            transition: all .3s ease;
            top: 0;
            height: 100vh;
            background: #000;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;

            .links {
                display: flex;
                flex-direction: column;
                gap: 30px;
                margin-bottom: 40px;
                text-align: center;
            }

            .cash {
                display: flex;
                justify-content: center;
                gap: 10px;
                align-items: center;
                margin-bottom: 40px;
                text-decoration: none;

            }

            a {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--geo);
                color: #fff;
                text-decoration: none;
            }
        }
    }

    .header__pc {
        width: 100%;
        padding: 25px 7.813vw;
        background: rgba(0, 0, 0, 0.80);
        backdrop-filter: blur(10px);

        @media (max-width: 1600px) {
            padding: 25px 30px;
        }

        @media (max-width: 1024px) {
            display: none;
        }

        display: flex;
        justify-content: space-between;

        .user {
            display: flex;
            gap: 40px;
            align-items: center;

            @media (max-width: 1300px) {
                gap: 30px;
            }

            .cash {
                display: flex;
                gap: 10px;
                align-items: center;
                text-decoration: none;

                span {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--geo);
                    color: #fff;
                }
            }
        }

        .links {
            display: flex;
            gap: 60px;
            align-items: center;

            @media (max-width: 1400px) {
                gap: 30px;
            }
        }

        a {
            text-decoration: none;
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            text-transform: uppercase;
            font-family: var(--geo);
            color: #fff;

            @media (max-width: 1200px) {
                font-size: 16px;
            }
        }
    }
}

.cartOpen {
    right: 0 !important;

    @media (max-width: 1024px) {
        transform: scale(1) !important;
    }
}

.cart {
    width: 600px;
    height: 100vh;
    position: fixed;
    right: -3000px;
    z-index: 250;
    background: #431E1A;
    backdrop-filter: blur(2px);
    transition: all .3s ease;
    padding: 99px 85px 50px 35px;

    @media (max-width: 1024px) {
        width: 100%;
        padding: 20px 11px 28px;
        margin: 130px 0px 0 0px;
        transform: scale(0);
        right: 0;
    }

    hr {
        border-top: 2px solid #292238;
    }

    .closespan {
        font-size: 20px !important;
        font-style: normal;
        font-weight: 400 !important;
        line-height: 130%;
        text-decoration-line: underline;
        font-family: var(--geo);
        cursor: pointer;
        color: #fff !important;
    }

    .cart__footer {
        p {
            font-size: 20px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            /* 26px */
            letter-spacing: 1px;
            text-transform: uppercase;
            margin: 0 0 20px;
            color: #fff;
            font-family: var(--geo);
        }

        .btns {
            display: flex;
            justify-content: center;
            gap: 36px;

            button,
            a {
                border-radius: 10px;
                background: var(--LightRed, #FF1D00);
                border: 0;
                text-decoration: none;
                text-align: center;
                display: inline-block;
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;
                padding: 12px 29px;
                transition: all .3s ease;

            }
        }
    }

    .cart__body {
        display: flex;
        flex-direction: column;
        gap: 30px;
        margin-top: 40px;
        width: 100%;
        height: 500px;
        overflow-y: auto;

        .cart__item {
            display: flex;
            gap: 20px;
            align-items: center;
            width: 100%;

            .delete {
                display: flex;
                justify-content: space-between;
                align-items: flex-start;
                width: 100%;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 300;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 12px;
                }
            }

            span {
                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                display: block;
                margin-top: 15px;
                font-family: var(--geo);
                color: #fff;
            }

            .img {
                min-width: 180px;
                height: 130px;
                background: #000;

                @media (max-width: 1024px) {
                    min-width: 130px;
                    height: 96px;
                }

                img {
                    width: 100%;
                    height: 100%;
                    object-fit: cover;
                }
            }
        }
    }
}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    margin-top: 10px;
    display: flex;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 35px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #fff;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}
</style>