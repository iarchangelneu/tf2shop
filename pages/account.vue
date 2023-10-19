<template>
    <div class="page">
        <PrevPage></PrevPage>

        <h1>Личный кабинет</h1>

        <div class="navs">
            <span :class="{ activeNav: activeNav == 0 }" @click="activeNav = 0">Личные данные</span>
            <span :class="{ activeNav: activeNav == 1 }" @click="activeNav = 1">Транзакции</span>
            <span :class="{ activeNav: activeNav == 2 }" @click="activeNav = 2">Баланс аккаунта</span>
        </div>

        <div class="account" v-if="activeNav == 0">
            <div class="account__left">
                <div class="balance__info">
                    <div class="balance">
                        <div>
                            <h2>Баланс аккаунта</h2>
                            <h3 v-if="userData.length <= 0"></h3>
                            <h3 v-else>{{ userData.balance.toFixed(1).toLocaleString() + ' ₸' }}</h3>
                        </div>

                        <div class="btns">
                            <NuxtLink to="/refill">Пополнить</NuxtLink>
                            <NuxtLink to="/withdrawal">Вывести</NuxtLink>
                        </div>

                    </div>
                    <div class="user">
                        <h3>{{ userData.steam_personaname || userData.first_name }}</h3>
                        <a :href="userData.steam_profileurl" v-if="userData.steam_profileurl" target="_blank">
                            <img src="@/assets/img/steamgray.svg" alt="">
                            <span>{{ userData.steam_personaname }}</span>
                        </a>
                    </div>
                </div>

                <div class="password">
                    <h2>Пароль</h2>
                    <div>
                        <label for="new-passwrod">Новый пароль</label>
                        <input type="password" name="new-passwrod" id="new-passwrod" v-model="password">
                    </div>
                    <div>
                        <label for="repeat-password">Повторите пароль</label>
                        <input type="password" name="repeat-password" id="repeat-password" v-model="repeat_password">
                    </div>

                    <button @click="editUser()">Сохранить пароль</button>
                    <button @click="logOut()" class="d-block mt-4">Выйти</button>
                </div>
            </div>

            <div class="account__right">
                <div>
                    <div class="label">
                        <div>
                            <img src="@/assets/img/link.svg" alt="">
                            <label for="link">Трейд-ссылка</label>
                        </div>
                        <NuxtLink to="https://steamcommunity.com/id/iarchangel/tradeoffers/privacy">Где получить?</NuxtLink>
                    </div>
                    <input type="url" name="link" id="link" placeholder="Вставьте вашу trade-ссылку" v-model="trade_link">

                    <button @click="editUser()">Сохранить</button>
                </div>

                <div class="email">
                    <div>
                        <label for="email">
                            E-mail
                        </label>
                        <input type="email" name="email" id="email" v-model="email">

                        <button @click="editUser()">Изменить</button>
                    </div>
                </div>
            </div>
        </div>

        <div class="transactions" v-if="activeNav == 1">
            <div class="empty" v-if="transactions.length <= 0">
                <h1>Список операций будет доступен, когда Вы пополните или выведете средства с баланса</h1>

                <div class="buttons">
                    <NuxtLink to="/catalog">Магазин</NuxtLink>
                    <NuxtLink to="/sale">Продажа</NuxtLink>
                </div>
            </div>
            <div class="scrollik" v-else>

                <div class="mobtrans">
                    <div class="mobbody">
                        <div class="mob__item" v-for="item in transactions">
                            <div class="imageblock">
                                <div class="counts">
                                    <small>Тип</small>
                                    <span>Покупка</span>
                                </div>
                                <small>Предмет</small>
                                <div class="item text-left">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>

                                </div>
                            </div>
                            <div class="countsblock">

                                <div class="counts">
                                    <small>Цена</small>
                                    <span>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                                </div>

                                <div class="counts">
                                    <small>Дата</small>
                                    <span>{{ formatDate(item.date) }}</span>
                                </div>
                                <div class="counts">
                                    <small>Статус</small>
                                    <span
                                        :class="{ process: item.status == 'В процессе', success: item.status == 'Совершено', failure: item.status == 'Отменено' }">{{
                                            item.status }}</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Предмет</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата</th>
                            <th scope="col">Статус</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in transactions">
                            <td>{{ item.type }}</td>
                            <td class="d-flex justify-content-center">
                                <div class="transitem">
                                    <img :src="item.item.img" alt="">
                                    <span>{{ item.item.name }}</span>

                                </div>
                            </td>
                            <td>{{ item.item.sell_price.toFixed(1).toLocaleString() }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td
                                :class="{ process: item.status == 'В процессе', success: item.status == 'Совершено', failure: item.status == 'Отменено' }">
                                {{ item.status }}</td>
                        </tr>

                    </tbody>
                </table>
            </div>
        </div>

        <div class="balance" v-if="activeNav == 2">
            <div class="empty" v-if="balance.length <= 0">
                <h1>Список операций будет доступен, когда Вы пополните или выведете средства с баланса</h1>
            </div>
            <div class="scrollik" v-else>

                <div class="mobtrans">

                    <div class="mob__body" v-for="item in balance.slice().reverse()" :key="item.id">
                        <div class="paddingblya">
                            <div class="body__header">
                                <small>{{ item.type_operation }}</small>
                                <span>{{ formatDate(item.date) }}</span>
                                <img src="@/assets/img/success.svg" v-if="item.paid == 1" alt="">
                                <img src="@/assets/img/failure.svg" v-else alt="">
                            </div>
                            <div class="body__footer">
                                <div>
                                    <small>Цена</small>
                                    <span> {{ item.amount.toLocaleString() }} ₸</span>
                                </div>
                                <div>
                                    <small>Состояние счета</small>
                                    <span>{{ item.amount_now.toFixed(1) }} ₸</span>
                                </div>
                            </div>
                        </div>
                        <hr>
                    </div>
                </div>
                <table class="table table-borderless text-center">
                    <thead>
                        <tr>
                            <th scope="col">Тип</th>
                            <th scope="col">Цена</th>
                            <th scope="col">Дата и время</th>
                            <th scope="col">Статус</th>
                            <th scope="col">Состояние счета</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in balance.slice().reverse()" :key="item.id">
                            <td>{{ item.type_operation }}</td>
                            <td> {{ item.amount }} ₸</td>
                            <td>{{ formatDate(item.date) }}</td>
                            <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ?
                                'Совершено'
                                : 'Отменено' }}</td>
                            <td>{{ item.amount_now.toFixed(1) }} ₸</td>
                        </tr>

                    </tbody>
                </table>
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
            activeNav: 0,
            userData: [],
            trade_link: '',
            email: '',
            password: '',
            repeat_password: '',
            transactions: [],
            balance: [],
            pathUrl: 'https://tf2shop.kz',
        }
    },
    methods: {
        formatDate(dateTime) {
            const date = new Date(dateTime);

            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
        editUser() {
            const url = `${this.pathUrl}/api/users/edit-profile/${this.userData.id}`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            if (this.password == '') {
                axios
                    .put(url, {
                        email: this.email,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
            else {
                axios
                    .put(url, {
                        email: this.email,
                        password: this.password,
                        trade_link: this.trade_link,

                    })

                    .then((res) => {
                        console.log(res)

                        this.getUser()
                    })
            }
        },
        getUser() {
            const url = `${this.pathUrl}/api/users/profile/`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.userData = res.data
                    this.trade_link = res.data.trade_link
                    this.email = res.data.email
                    this.transactions = res.data.transactions_item
                    this.balance = res.data.transactions_balance
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getUser()

        }
        else {
            this.$router.push('/register')
        }

    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Личный кабинет | DotaShop',
    ogTitle: 'Личный кабинет | DotaShop',
    description: 'Личный кабинет | DotaShop',
    ogDescription: 'Личный кабинет | DotaShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 130px 70px 100px;

    @media (max-width: 1600px) {
        padding: 130px 50px 100px;
    }

    @media (max-width: 1024px) {
        padding: 130px 20px 50px;
    }

    .success {
        color: #05EA2A !important;
    }

    .failure {
        color: #EA0505 !important;
    }

    .process {
        color: #EAE105 !important;
    }

    .tradelock {
        color: #EF8641 !important;
    }

    .balance {
        margin-top: 37px;
        // padding: 30px;
        border-radius: 10px;
        background: #2A1310;

        hr {
            margin: 20px 0;
            border-top: 1px solid #fff;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 500px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                text-transform: none;
                color: #fff;
            }
        }

        .scrollik {
            height: 700px;
            overflow-y: auto;

        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #FF1D00;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #000;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            // border-radius: 30px;
            // background: #FFF;
            // box-shadow: 0px 0px 15px 0px rgba(47, 59, 163, 0.20);

            .mob__body {

                &:last-child {
                    hr {
                        display: none;
                    }
                }

                .paddingblya {

                    // padding: 30px;


                    .body__header {
                        display: flex;
                        justify-content: space-between;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--geo);
                            color: #fff;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--geo);
                            color: #fff;
                            white-space: nowrap;
                        }
                    }

                    .body__footer {
                        display: flex;
                        justify-content: space-between;
                        margin-top: 15px;
                        text-align: left;

                        small {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 500;
                            line-height: normal;
                            font-family: var(--geo);
                            color: #fff;
                            text-align: left;
                        }

                        span {
                            font-size: 16px;
                            font-style: normal;
                            font-weight: 400;
                            line-height: normal;
                            font-family: var(--geo);
                            color: #fff;
                            display: block;
                            text-align: left;
                            margin-top: 10px;
                            //   white-space: nowrap;
                        }

                    }
                }
            }

            @media (max-width: 1024px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1024px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .item {
                    max-width: 130px;
                    padding: 5px;
                    display: flex;
                    flex-direction: column;
                    border-radius: 10px;
                    border: 1.038px solid var(--Rare, #3348E0);
                    background: linear-gradient(90deg, #24212B 0%, rgba(36, 33, 43, 0.65) 100%);

                    span {
                        font-size: 8.306px;
                        font-style: normal;
                        font-weight: 600;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        margin: 5px 0 5px;

                        &:last-child {
                            margin: 0;
                        }
                    }

                    img {
                        width: 119px;
                        height: 79px;
                    }
                }
            }
        }
    }

    .transactions {
        padding: 20px 30px;
        border-radius: 10px;
        margin-top: 37px;

        @media (max-width: 1400px) {
            padding: 10px;
        }

        .empty {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 500px;

            h1 {
                font-size: 24px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                text-transform: none;
                color: #fff;
                margin: 0;

                @media (max-width: 1024px) {
                    font-size: 18px;
                }
            }

            .buttons {
                display: flex;
                gap: 30px;
                margin-top: 50px;

                a {
                    border-radius: 10px;
                    border: 0;
                    background: var(--iris-100, #FF1D00);
                    padding: 10px 3.802vw;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 130%;
                    font-family: var(--geo);
                    transition: all .3s ease;
                    color: #fff;
                    text-decoration: none;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }

                    &:last-child {
                        border: 0;
                        background: #0500FF;


                    }
                }
            }
        }

        .scrollik {
            height: 700px;
            overflow-y: auto;

            @media (max-width: 1400px) {
                overflow-y: unset;
                height: auto;
            }
        }

        .scrollik {
            scrollbar-color: #999 #333;
        }

        .scrollik::-webkit-scrollbar {
            width: 10px;
            height: 10px;
        }

        .scrollik::-webkit-scrollbar-thumb {
            background: #0500FF;
            border-radius: 10px;
        }

        .scrollik::-webkit-scrollbar-track {
            background: #2A1310;
            border-radius: 10px;
        }

        .mobtrans {
            display: none;

            .mobbody {
                display: flex;
                flex-direction: column;
                gap: 20px;

                .mob__item {
                    display: flex;
                    gap: 40px;
                    justify-content: space-between;
                    flex-direction: column;

                    .countsblock {
                        display: flex;
                        flex-direction: column;
                        gap: 20px;
                    }

                    .imageblock {
                        .counts {
                            margin-bottom: 17px;
                        }

                        .item {
                            margin-top: 15px;

                            img {
                                width: 145px;
                                height: 110px;
                            }
                        }
                    }

                    small,
                    span {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--geo);
                        color: #fff;

                        display: block;
                    }





                    .counts {
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        width: 100%;
                    }
                }
            }

            @media (max-width: 1400px) {
                display: block;
            }
        }

        table {
            @media (max-width: 1400px) {
                display: none;
            }

            th {
                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;
            }

            .success {
                color: #05EA2A !important;
            }

            .failure {
                color: #EA0505 !important;
            }

            .process {
                color: #EAE105 !important;
            }

            .tradelock {
                color: #EF8641 !important;
            }

            td {
                font-size: 16px;
                font-style: normal;
                font-weight: 300;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1600px) {
                    font-size: 16px;
                }

                .transitem {
                    display: flex;
                    gap: 10px;
                    align-items: center;

                    img {
                        width: 145px;
                        height: 110px;
                    }

                    span {
                        max-width: 258px;
                    }
                }
            }
        }

        .filters {
            display: flex;
            justify-content: space-between;

            @media (max-width: 1400px) {
                flex-direction: column;
                gap: 15px;
            }

            .shit {
                display: flex;
                gap: 64px;

                @media (max-width: 1400px) {
                    flex-direction: column;
                    gap: 15px;
                }

                .buttons {
                    display: flex;
                    gap: 30px;

                    button {
                        padding: 10px 20px;
                        border-radius: 10px;
                        border: 0;
                        background: var(--iris-100, #FF1D00);

                        font-size: 16px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        transition: all .3s ease;

                        @media (max-width: 1400px) {
                            padding: 10px 0;
                            flex: 1;
                        }

                        &:hover {
                            border: 0;
                            background: #0500FF;
                        }

                        &:last-child {
                            border-radius: 10px;
                            border: 0;
                            background: #0500FF;

                            &:hover {
                                border: 0;
                                background: var(--iris-100, #FF1D00);
                            }
                        }
                    }
                }

                .selects {
                    display: flex;
                    gap: 40px;

                    @media (max-width: 1400px) {
                        gap: 15px;
                        flex-direction: column;
                    }
                }

                select {
                    background: transparent;
                    color: #fff;
                    border: 0;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: 32px;
                    font-family: var(--geo);
                    color: #fff;

                    @media (max-width: 1400px) {
                        max-width: 350px;
                    }
                }
            }

            input {
                border-radius: 10px;
                background: #2A1310;
                padding: 10px 20px 10px 0;
                border: 2px solid #FF1D00;
                border-left: 0;
                border-top-left-radius: 0;
                border-bottom-left-radius: 0;
                max-width: 590px;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 32px;
                font-family: var(--geo);
                color: #fff;
                box-shadow: none;

                @media (max-width: 1400px) {
                    max-width: 100%;
                }

                &::placeholder {
                    color: #fff;
                }
            }

            .input-group-text {
                border-radius: 10px;
                background: #2A1310;
                border: 2px solid #FF1D00;
                border-right: 0;
                border-top-right-radius: 0;
                border-bottom-right-radius: 0;
            }
        }
    }

    .account {
        margin-top: 37px;
        display: flex;
        transition: all .3s ease;
        gap: 8.333vw;
        justify-content: space-between;
        align-items: flex-start;

        @media (max-width: 1300px) {
            flex-direction: column;
            gap: 35px;
        }

        label {
            margin: 0 0 9px;
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--geo);
            color: #fff;
        }

        input {
            border: 0;
            width: 100%;
            border-radius: 5px;
            background: #2A1310;
            padding: 12px 15px;
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--geo);
            color: #fff;
            margin-bottom: 25px;

            &::placeholder {
                color: #fff;
            }

            @media (max-width: 1024px) {
                margin-bottom: 20px;
            }
        }

        button {
            padding: 10px 61px;
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
                font-size: 16px;
            }
        }


        .account__right {
            .email {
                margin-top: 30px;

                input {
                    max-width: 573px;
                    display: block;

                    @media (max-width: 1300px) {
                        max-width: 100%;
                    }
                }
            }

            label {
                font-size: 24px;
                font-family: var(--geo);
                font-weight: 400;
                line-height: normal;
                letter-spacing: 1.2px;

                @media (max-width: 1024px) {
                    font-size: 16px;
                }

            }

            width: 100%;


            .label {
                width: 100%;
                display: flex;
                justify-content: space-between;
                margin: 0 0 10px;

                div {
                    display: flex;
                    align-items: center;
                    gap: 10px;

                    label {
                        margin: 0;
                    }


                }

                a {
                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    text-decoration-line: underline;
                    font-family: var(--geo);
                    color: #fff;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }
            }
        }

        .account__left {
            width: 100%;

            .password {
                h2 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    letter-spacing: 1.2px;
                    font-family: var(--geo);
                    color: #fff;
                    margin: 30px 0 18px;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                        margin: 20px 0;
                    }
                }



                label,
                input {
                    display: block;
                }
            }

            .user {
                display: flex;
                gap: 40px;

                h3 {
                    font-size: 24px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    letter-spacing: 1.2px;
                    text-transform: uppercase;
                    font-family: var(--geo);
                    color: #fff;
                    margin: 0;

                    @media (max-width: 1024px) {
                        font-size: 16px;
                    }
                }

                a {
                    display: flex;
                    align-items: center;
                    gap: 10px;
                    text-decoration: none;

                    span {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 300;
                        line-height: normal;
                        font-family: var(--geo);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }
                }
            }

            .balance__info {
                display: flex;
                flex-direction: column-reverse;
                gap: 20px;

                @media (max-width: 1300px) {
                    flex-direction: column;
                }

                .balance {
                    margin: 0;
                    border-radius: 10px;
                    border: 2px solid var(--LightRed, #FF1D00);
                    background: #2A1310;
                    padding: 43px 62px 41px 66px;
                    text-align: center;
                    width: 100%;
                    justify-content: space-between;
                    display: flex;

                    @media (max-width: 1300px) {
                        padding: 10px 15px;
                    }

                    @media (max-width: 1024px) {
                        flex-direction: column;
                        justify-content: center;
                        align-items: center;
                    }

                    a {
                        text-decoration: none;
                        display: block;
                        border-radius: 10px;
                        border: 0;
                        background: var(--iris-100, #0500FF);
                        padding: 13px 45px;

                        font-size: 20px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        transition: all .3s ease;
                        margin: 0 0 20px;
                        width: 100%;




                        &:last-child {
                            margin: 0;
                            width: 100%;
                            border-radius: 10px;
                            border: 0;
                            background: #FF1D00;



                        }
                    }

                    h3 {
                        font-size: 48px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;

                        @media (max-width: 1300px) {
                            font-size: 20px;
                        }
                    }

                    h2 {
                        font-size: 24px;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                        font-family: var(--geo);
                        color: #fff;
                        margin: 0 0 24px;

                        @media (max-width: 1300px) {
                            font-size: 16px;
                            text-align: left;
                        }

                    }
                }
            }
        }
    }

    .navs {
        display: flex;
        justify-content: center;
        gap: 50px;

        @media (max-width: 1024px) {
            justify-content: flex-start;
            gap: 20px;
            flex-wrap: wrap;
        }

        .activeNav {
            color: #fff !important;
        }

        span {
            display: block;
            font-size: 24px;
            font-style: normal;
            font-weight: 500;
            line-height: normal;
            letter-spacing: 1.2px;
            font-family: var(--geo);
            color: #C8C8C8;
            cursor: pointer;
            transition: all .3s ease;

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
        /* 41.6px */
        letter-spacing: 1.6px;
        text-transform: uppercase;
        font-family: var(--fun);
        color: #fff;
        margin: 27px 0 30px;

        @media (max-width: 1024px) {
            font-size: 24px;
        }
    }
}
</style>