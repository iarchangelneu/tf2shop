<template>
    <div class="page">
        <PrevPage></PrevPage>

        <div class="form">
            <div class="form__body">
                <h1 class="text-center">
                    Пополнение баланса
                </h1>

                <div class="type">
                    <NuxtLink to="/refill">Пополнить баланс</NuxtLink>
                    <NuxtLink to="/withdrawal">Вывести средства</NuxtLink>
                </div>


                <h2>Порядок действий для пополнения средств</h2>


                <div class="footer">
                    <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>

                    <div>
                        <input type="radio" id="terms">
                        <label for="terms">Я согласен с <NuxtLink to="/terms"> пользовательским соглашением</NuxtLink>
                            и <NuxtLink to="/terms">политикой конфиденциальности</NuxtLink></label>
                    </div>


                    <p class="mt-4">2. Введите сумму, на которую Вы хотите пополнить личный счет, и нажмите на кнопку
                        “Пополнить”. Вы будете переадресованы на сайт платежной системы, где сможете завершить платеж.</p>

                    <div class="pay">
                        <input type="number" v-model="cost">
                        <button ref="inBtn" @click="inMoney()">Пополнить</button>
                    </div>

                    <div class="select">
                        <button :class="{ active: cost == 100 }" @click="cost = 100">100 ₸</button>
                        <button :class="{ active: cost == 1000 }" @click="cost = 1000">1000 ₸</button>
                        <button :class="{ active: cost == 2000 }" @click="cost = 2000">2000 ₸</button>
                        <button :class="{ active: cost == 5000 }" @click="cost = 5000">5000 ₸</button>
                        <button :class="{ active: cost == 10000 }" @click="cost = 10000">10000 ₸</button>
                    </div>
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
            payType: '',
            cost: null,
            pathUrl: 'https://tf2shop.kz',

        }
    },

    methods: {
        inMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/new-pay`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.inBtn.innerHTML = 'Ожидайте'

            axios
                .post(path, {
                    amount: this.cost
                })
                .then(response => {
                    console.log(response)
                    window.location.href = response.data.url
                    if (response.status = 201) {
                        this.$refs.inBtn.innerHTML = 'Пополнить'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }
                })
                .catch(error => {
                    console.error(error)
                    this.$refs.inBtn.innerHTML = 'Пополнить'
                })
        },
    },

}
</script>
<script setup>
useSeoMeta({
    title: 'Пополнение баланса | TF2Shop',
    ogTitle: 'Пополнение баланса | TF2Shop',
    description: 'Пополнение баланса | TF2Shop',
    ogDescription: 'Пополнение баланса | TF2Shop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 150px 0 0 200px;
    // height: 100%;
    position: relative;

    @media (max-width: 1024px) {
        padding: 150px 20px 50px;
    }

    .bg {
        position: absolute;
        // width: 743px;
        // height: 685px;
        right: 0;
        bottom: -1.5%;
        z-index: 1;

        @media (max-width: 1024px) {
            display: none;
        }

    }

    .form {
        z-index: 2;
        margin-top: 20px;
        padding-bottom: 140px;
        display: flex;
        justify-content: center;
        // max-width: 680px;
        position: relative;
        margin-right: 190px;

        @media (max-width: 1024px) {
            margin-right: 0;
            padding: 0;
        }

        .cardd {
            display: flex;
            flex-direction: column;

            input {
                display: block;
                border-radius: 10px;
                border: 2px solid #FF1D00;
                padding: 12px 10px 9px;
                background: transparent;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;

                font-family: var(--geo);
                color: #fff;
            }
        }

        .select {
            margin-top: 20px;
            display: flex;
            justify-content: center;
            gap: 11.94px;
            flex-wrap: wrap;

            .active {
                background: #FF1D00 !important;
            }

            button {
                border-radius: 10px;
                border: 2px solid #FF1D00;
                background: transparent;
                padding: 11px 18px;
                transition: all .3s ease;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }
            }
        }

        .pay {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;

            input {
                flex: 1;
                text-align: right;
                border-radius: 10px;
                border: 2px solid #FF1D00;
                padding: 12px 10px 9px;
                background: transparent;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;

                font-family: var(--geo);
                color: #fff;
                max-width: 220px;

                @media (max-width: 1024px) {
                    max-width: 155px;
                }
            }

            button {
                flex: 1;
                max-width: 220px;
                padding: 10px 0;

                border-radius: 10px;
                border: 0;
                background: var(--iris-100, #0500FF);

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    max-width: 155px;
                }
            }
        }

        .footer {
            p {
                margin: 0 0 20px;
            }

            div {
                display: flex;
                gap: 10px;

                label {
                    margin: 0;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: normal;
                    font-family: var(--geo);
                    color: #fff;
                    max-width: 440px;

                    a {
                        color: #fff;
                        text-decoration: underline;
                    }
                }
            }
        }

        .paytype {
            p {
                margin-top: 0;
                margin-bottom: 20px;
            }

            div {
                display: flex;
                gap: 10px;
                margin-bottom: 20px;

                label {
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 300;
                    line-height: normal;
                    font-family: var(--geo);
                    color: #fff;
                    margin: 0;
                }
            }
        }

        h2 {
            margin: 30px 0 20px;
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            text-align: center;
            font-family: var(--geo);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 16px;
                margin: 20px 0;
            }
        }

        p {
            max-width: 600px;
            font-size: 16px;
            font-style: normal;
            font-weight: 300;
            line-height: 130%;
            font-family: var(--geo);
            color: #fff;
            margin: 15px 0 0;
        }

        .type {
            margin-top: 25px;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;

            a {
                text-decoration: none;
                text-align: center;
                flex: 1;
                max-width: 229px;
                padding: 10px 0;
                border-radius: 10px;
                background: #0500FF;
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 12px;
                }

                &:last-child {
                    border-radius: 10px;
                    background: #292854;
                }
            }
        }

        h1 {
            font-size: 32px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            letter-spacing: 1.6px;
            font-family: var(--fun);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 20px;
            }
        }

        .form__body {
            border-radius: 10px;
            border: 2px solid var(--LightRed, #FF1D00);
            background: #2A1310;
            padding: 30px 40px 48px;

            @media (max-width: 1024px) {
                padding: 20px 10px;
            }
        }
    }
}
</style>