<template>
    <div class="page">
        <div class="form">
            <div class="text-center">
                <img src="@/assets/img/reglogo.svg" class="img-fluid" alt="">
            </div>

            <h1 class="text-center">
                Авторизация
            </h1>


            <div class="inputs">
                <div>
                    <label for="email">Ваш e-mail</label>
                    <input type="email" name="email" id="email" placeholder="E-mail" v-model="email">
                </div>
                <div>
                    <label for="password">Пароль</label>
                    <div class="showpass">
                        <input :type="showPassword ? 'text' : 'password'" name="password" id="password"
                            placeholder="Введите пароль" v-model="password">
                        <img @click="togglePasswordVisibility('password-repeat')" src="@/assets/img/showpass.svg" alt=""
                            srcset="" />
                    </div>
                </div>
                <small>{{ error }}</small>
                <div class="btns">
                    <button @click="login">Войти</button>
                    <button @click="loginSteam">Войти через Steam</button>
                </div>


                <div class="text-center">
                    <span>Нет аккаунта? <NuxtLink to='/register'>Зарегистрироваться</NuxtLink></span>
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
            showPassword: false,
            email: '',
            password: '',
            pathUrl: 'https://tf2shop.kz',
            error: '',
        }
    },
    methods: {
        login() {
            const url = `${this.pathUrl}/api/users/authorization`

            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;

            axios
                .post(url, { username: this.email, password: this.password })
                .then((res) => {



                    document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                    localStorage.setItem('accountType', 'email')

                    window.location.href = '/account'


                    console.log(res)
                })
                .catch((error) => {
                    console.log(error);
                    this.error = error.response.data.non_field_errors.toString()
                });
        },
        loginSteam() {
            const url = `${this.pathUrl}/api/users/login-steam/`

            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    window.location.href = res.data.url
                })
                .catch((error) => {
                    this.error = error.response.data.detail
                    console.log(error.response);
                });
        },
        togglePasswordVisibility(inputId) {
            const inputElement = document.getElementById(inputId);
            if (inputElement) {
                this.showPassword = !this.showPassword;
                inputElement.type = this.showPassword ? 'text' : 'password';
            }
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.$router.push('/account')

        }
        else {

        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Авторизация | TF2Shop',
    ogTitle: 'Авторизация | TF2Shop',
    description: 'Авторизация | TF2Shop',
    ogDescription: 'Авторизация | TF2Shop',
})
</script>
<style lang="scss" scoped>
.page {
    background: url('@/assets/img/reg.png') no-repeat;
    display: flex;
    justify-content: center;
    align-items: center;

    @media (max-width: 1024px) {
        padding: 0;
    }

    small {
        color: red;
        font-family: var(--mon);
        font-size: 14px;
    }

    .form {
        height: 100vh;
        padding: 50px 54px 21px;
        max-width: 600px;

        @media (max-width: 1024px) {
            height: 100vh;
            max-width: 100%;
            width: 100%;
            padding: 30px 10px;
            border-radius: 0;
        }

        border-radius: 0;
        background: #431E1A;
        backdrop-filter: blur(2px);

        .showpass {
            position: relative;

            img {
                position: absolute;
                right: 3%;
                top: 25%;
                cursor: pointer;

                @media (max-width: 1024px) {
                    top: 20%;
                }
            }
        }

        .inputs {
            margin-top: 30px;
            padding: 0 20px;

            @media (max-width: 1024px) {
                padding: 0;
            }

            .btns {
                display: flex;
                gap: 20px;
            }

            button {
                width: 100%;
                padding: 11.5px 0;
                border-radius: 10px;
                background: var(--LightRed, #FF1D00);
                border: 0;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }

                &:last-child {
                    background: #0500FF;
                }
            }

            span {
                margin-top: 20px;
                font-size: 20px;
                font-style: normal;
                font-weight: 300;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    font-size: 12px;
                }

                a {
                    color: #fff;
                    text-decoration: underline;
                }
            }
        }

        input {
            width: 100%;
            margin-bottom: 30px;
            border-radius: 10px;
            border: 2px solid var(--LightRed, #FF1D00);
            background: #2A1310;
            padding: 14px 15px;

            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: 24px;
            font-family: var(--geo);
            color: #fff;

            @media (max-width: 1024px) {
                margin-bottom: 20px;
                font-size: 16px;
                padding: 8px 15px;
            }
        }

        label {
            font-size: 24px;
            font-style: normal;
            font-weight: 400;
            line-height: 24px;
            /* 100% */
            letter-spacing: 1.2px;
            font-family: var(--geo);
            color: #fff;
            margin: 0 0 10px;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }

        label,
        input,
        span {
            display: block;
        }

        .steam {
            margin-top: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 11.5px 23px;
            border-radius: 10px;
            border: 1px solid var(--iris-100, #5D5FEF);
            background: #252654;

            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--geo);
            color: #fff;
        }

        h1 {
            margin: 67px 0 20px;
            font-size: 48px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            /* 41.6px */
            letter-spacing: 1.6px;

            font-family: var(--fun);
            color: #fff;

            @media (max-width: 1024px) {
                margin: 23px 0 10px;
                font-size: 20px;
            }
        }

        small {
            text-align: center;
            display: block;
            font-size: 16px;
            font-style: normal;
            font-weight: 300;
            line-height: 130%;
            font-family: var(--geo);
            color: #fff;
            margin: 0 0 10px;

            @media (max-width: 1024px) {
                font-size: 12px;
            }
        }
    }
}
</style>