<template>
    <div class="page">
        <div class="main">
            <div class="wrapper">
                <h1 class="text-center">
                    Выделяйся со скинами на Team Fortress 2
                </h1>

                <div class="buttons">
                    <div>
                        <img src="@/assets/img/main1.png" alt="">
                        <NuxtLink to="/catalog">Перейти в магазин</NuxtLink>
                    </div>
                    <div>
                        <img src="@/assets/img/main2.png" alt="">
                        <NuxtLink to="/sale" class="sale">Продать предметы</NuxtLink>
                    </div>
                </div>
            </div>
        </div>

        <div class="paddingpage">
            <div class="populars">
                <h1>
                    <span>Популярное</span>
                    <img src="@/assets/img/title.svg" alt="">
                </h1>

                <div v-if="catalog.length <= 0"></div>
                <div class="catalog__items" v-else>
                    <NuxtLink v-for="item in catalog.results.slice(0, 6)" :key="item.id" :to="'/product/' + item.id"
                        class="catalog__item">
                        <img :src="item.img" alt="" class="item__image">
                        <p>{{ item.name }}</p>
                        <span>{{ item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                    </NuxtLink>
                </div>
                <div class="showmore">
                    <NuxtLink to="/catalog">Посмотреть все</NuxtLink>
                </div>
            </div>
        </div>
        <div class="skins">
            <h1 class="text-center">
                Одень персонажей в <span>уникальном</span> стиле
            </h1>

            <div class="skins__slider">
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin1.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin2.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>
                <div class="skin-image"><img src="@/assets/img/skin3.png" alt=""></div>

            </div>
        </div>

        <div class="paddingpage">
            <div class="populars">
                <h1>
                    <span>Акции и скидки</span>
                    <img src="@/assets/img/title.svg" alt="">
                </h1>

                <div v-if="catalog.length <= 0"></div>
                <div class="catalog__items" v-else>
                    <NuxtLink v-for="item in catalog.results.slice(7, 13)" :key="item.id" :to="'/product/' + item.id"
                        class="catalog__item">
                        <small>-{{ Math.floor(100 - (item.sell_price / item.price * 100)) }}%</small>
                        <img :src="item.img" alt="" class="item__image">
                        <p>{{ item.name }}</p>
                        <div class="price">
                            <small>{{ item.price.toFixed(1).toLocaleString() }} ₸</small>
                            <span>{{ item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                        </div>
                    </NuxtLink>
                </div>
                <a name="about"></a>
                <div class="showmore">
                    <NuxtLink to="/catalog">Посмотреть все</NuxtLink>
                </div>

            </div>

            <div class="about">
                <h1>О нас</h1>

                <div class="about__body">
                    <div class="about__left">

                        <img src="@/assets/img/aboutleft.png" class="img-fluid helpme" alt="">


                        <p>Наша цель — предоставить вам лучший выбор скинов для Team Fortress 2, чтобы вы могли выразить
                            свой стиль и уникальность в мире игры. Мы постоянно обновляем ассортимент, следим за редкими
                            предметами и предлагаем доступные цены для всех игроков.</p>
                    </div>
                    <div class="about__right">
                        <div>
                            <div class="chels">
                                <img src="@/assets/img/chel2.png" alt="">
                                <img src="@/assets/img/chel1.png" class="chel" alt="">
                            </div>
                            <p>С TF2SHOP.KZ Вы можете заработать, продавая свои скины на нашей платформе. Для этого просто
                                зарегистрируйтесь через аккаунт Steam и Ваш инвентарь автоматически подгрузится!</p>
                        </div>

                        <img src="@/assets/img/aboutright.png" class="img-fluid helpme" alt="">
                    </div>
                </div>
            </div>
        </div>

        <a name="faq"></a>
        <div class="faq">
            <div class="faq__body">
                <div class="faq__left">
                    <div class="title">
                        <h1>FAQ</h1>
                        <img src="@/assets/img/faqchel.png" alt="">
                    </div>

                    <button v-for="faq in  faqs " :key="faq.id" :data-target="'#collapseExample' + faq.id"
                        aria-expanded="false" :aria-controls="'collapseExample' + faq.id" @click="toggleCollapse(faq.id)">
                        <div class="inside">
                            <span :style="{ 'font-weight': faq.isOpen ? '600' : '300' }">
                                {{ faq.question }}
                            </span>
                            <img src="@/assets/img/plus.svg" :class="{ rotate: faq.isOpen }"
                                style="transition: all .3s ease;" alt="">
                        </div>

                        <div :class="{ collapse: !faq.isOpen }" style="transition:  all .3s ease;"
                            :id="'collapseExample' + faq.id">
                            <span class="mt-3 textik">Нет, к сожалению Steam не предоставляет никаких способов для вывода
                                средств с
                                их счета.</span>
                        </div>
                    </button>
                </div>
                <div class="faq__right">
                    <img src="@/assets/img/faqright.png" class="img-fluid" alt="">
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
            faqs: [
                { id: 1, isOpen: false, question: '1. Что делать, если после пополнения баланса деньги не поступили на счет?' },
                { id: 2, isOpen: false, question: '2. Что делать, если предмет не пришел или вы не можете его забрать?' },
                { id: 3, isOpen: false, question: '3. Что делать, если к вам в стим добавился “наш сотрудник” и предлагает вам двойной баланс за ваши вещи?' },
                { id: 4, isOpen: false, question: '4. Можно ли перевести средства со счета Steam?' },
                { id: 5, isOpen: false, question: '5. Как продать или вывести предмет с сайта?' },
            ],
            catalog: [],
            pathUrl: 'https://tf2shop.kz',
        }
    },
    methods: {
        toggleCollapse(id) {
            const faq = this.faqs.find(f => f.id === id);
            faq.isOpen = !faq.isOpen;
        },
        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
    mounted() {
        this.getCatalog()
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Главная | TF2Shop',
    ogTitle: 'Главная | TF2Shop',
    description: 'Главная | TF2Shop',
    ogDescription: 'Главная | TF2Shop',
})
</script>
<style lang="scss" scoped>
.page {

    .faq {
        padding: 0 0 0 70px;
        margin-top: 71px;

        @media (max-width: 1600px) {
            padding: 0 0 0 50px;
        }

        @media (max-width: 1024px) {
            padding: 0;
        }

        .faq__body {
            display: flex;
            align-items: flex-end;
            gap: 20px;

            @media (max-width: 1024px) {
                flex-direction: column;
            }

            .faq__right {
                width: 100%;
            }

            .faq__left {
                display: flex;
                flex-direction: column;
                gap: 30px;
                width: 100%;

                @media (max-width: 1024px) {
                    padding: 0 20px;
                    gap: 20px;
                }


                .rotate {
                    transform: rotate(40deg) !important;
                }

                button {
                    width: 100%;
                    // max-width: 730px;
                    text-align: left;
                    padding: 9px 15px 9px 10px;
                    z-index: 2;
                    background: transparent;
                    border: 0;
                    position: relative;
                    transition: all .3s ease;
                    outline: none;
                    box-shadow: none;

                    .inside {
                        display: flex;
                        justify-content: space-between;
                        align-items: center;
                        gap: 20px;
                        height: auto;

                    }

                    span {
                        font-size: 19px;
                        font-style: normal;
                        font-weight: 300;
                        line-height: 130%;
                        font-family: var(--geo);
                        color: #fff;
                        max-width: 686px;
                        display: block;
                        // transition: all .3s ease;

                        @media (max-width: 1024px) {
                            font-size: 16px;
                        }
                    }

                    .textik {
                        font-size: 16px !important;
                        font-weight: 300 !important;
                    }

                    .collapse {
                        display: none;
                        transition: height 0.3s ease-in-out;
                    }

                    .collapse.show {
                        display: block;
                    }
                }

                .title {
                    display: flex;
                    justify-content: space-between;
                    align-items: center;

                    h1 {
                        margin: 0;
                        font-size: 36px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: 130%;
                        font-family: var(--fun);
                        color: #fff;

                        @media (max-width: 1024px) {
                            font-size: 24px;
                        }
                    }
                }
            }
        }
    }

    .paddingpage {
        padding: 0 70px 0;

        @media (max-width: 1600px) {
            padding: 0 50px 0;
        }

        @media (max-width: 1024px) {
            padding: 0 20px 0;
        }

        .about {
            margin-top: 40px;

            @media (max-width: 1024px) {
                margin-top: 20px;
            }

            .about__body {
                display: flex;
                gap: 6px;
                padding: 0 0 0 59px;

                @media (max-width: 1600px) {
                    padding: 0;
                }

                @media (max-width: 1024px) {
                    flex-direction: column;
                    gap: 20px;
                }

                .helpme {
                    @media (max-width: 1600px) {
                        max-width: 50%;
                        max-height: 80%;
                    }
                }

                .about__right {
                    display: flex;
                    gap: 40px;

                    img {
                        @media (max-width: 1024px) {
                            display: none;
                        }
                    }

                    div {
                        display: flex;
                        flex-direction: column;
                        align-items: center;
                        justify-content: center;


                        .chels {
                            margin-left: -70px;
                            display: flex;
                            flex-direction: row;
                            gap: 30px;

                            @media (max-width: 1400px) {
                                margin: 0;
                                gap: 15px;
                            }



                            .chel {
                                transform: rotateY(180deg);
                            }
                        }

                        p {
                            font-size: 20px;
                            font-style: normal;
                            font-weight: 300;
                            line-height: 140%;
                            font-family: var(--geo);
                            color: #fff;
                            margin: 0;
                            max-width: 488px;

                            @media (max-width: 1024px) {
                                max-width: 100%;
                                font-size: 16px;
                            }
                        }
                    }
                }

                .about__left {
                    display: flex;

                    @media (max-width: 1024px) {
                        flex-direction: column-reverse;
                        justify-content: center;
                        align-items: center;
                        gap: 20px;
                    }


                    p {
                        font-size: 20px;
                        font-style: normal;
                        font-weight: 300;
                        line-height: 140%;
                        font-family: var(--geo);
                        color: #fff;
                        margin: 0;
                        max-width: 488px;

                        @media (max-width: 1024px) {
                            max-width: 100%;
                            font-size: 16px;
                        }
                    }

                    img {
                        margin-right: -40px;

                        @media (max-width: 1024px) {
                            margin: 0;
                            text-align: center;
                            max-width: 256px;
                            max-height: auto;
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

                @media (max-width: 1024px) {
                    font-size: 24px;
                    margin: 0 0 20px;
                }
            }
        }

        .populars {
            margin-top: 40px;

            .showmore {
                margin-top: 20px;
                display: flex;
                justify-content: right;

                a {
                    display: inline-block;
                    border-radius: 10px;
                    background: #FF1D00;
                    padding: 13px 15px;
                    text-decoration: none;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;

                    font-family: var(--geo);
                    color: #fff;

                }
            }

            .catalog__items {
                display: grid;
                grid-template-columns: repeat(auto-fill, minmax(279px, 1fr));
                gap: 21px;
                grid-auto-flow: dense;
                margin-top: 20px;

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

            h1 {
                display: block;
                position: relative;
                font-family: var(--fun);
                color: #fff;
                font-size: 36px;
                font-style: normal;
                font-weight: 500;
                line-height: 130%;

                @media (max-width: 1024px) {
                    font-size: 24px;
                }

                span {
                    z-index: 2;
                    position: relative;
                }

                img {
                    margin-top: -20px;
                    margin-left: -40px;
                    z-index: 1;

                    @media (max-width: 1024px) {
                        display: none;
                    }
                }
            }
        }
    }

    .skins {
        margin-top: 40px;
        overflow: hidden;

        .skins__slider {
            display: flex;
            gap: 96px;
            position: relative;
            animation: marquee 20s linear infinite;
        }

        .skin-image {
            max-width: 100%;
        }

        @keyframes marquee {
            0% {
                transform: translateX(0);
            }

            100% {
                transform: translateX(calc(-100% + 1rem));
            }
        }

        h1 {
            font-size: 36px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            text-transform: uppercase;
            font-family: var(--fun);
            color: #fff;

            @media (max-width: 1024px) {
                font-size: 20px;
                text-align: center;
                padding: 0 50px;
            }

            span {
                color: #FF1D00;
            }
        }
    }

    .main {
        height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background-image: url('@/assets/img/mainbg.png');
        background-repeat: no-repeat;
        background-size: 100vw 100vh;

        @media (max-width: 1024px) {
            background-size: 100% 800px;
            margin-top: 130px;
            height: auto;
        }

        @media (max-width: 500px) {
            background-size: 100% 217px;
        }

        .wrapper {
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            background: linear-gradient(180deg, rgba(0, 0, 0, 0.00) 51.85%, #000 97.22%);
        }

        .buttons {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-top: 34px;

            @media (max-width: 1024px) {
                img {
                    display: none;
                }

                margin-top: 110px;
            }

            div {
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;

                a {
                    border-radius: 10px;
                    background: #FF1D00;
                    box-shadow: 0px 4px 20px 0px rgba(0, 0, 0, 0.25);
                    padding: 15px 20px;
                    text-decoration: none;

                    font-size: 20px;
                    font-style: normal;
                    font-weight: 400;
                    line-height: normal;
                    font-family: var(--geo);
                    color: #fff;

                    @media (max-width: 1024px) {
                        flex: 1;
                        padding: 10px 5px;
                        text-align: center;
                        font-size: 16px;
                    }
                }

                .sale {
                    background: #0500FF;
                    box-shadow: 0px 4px 20px 0px rgba(0, 0, 0, 0.25);
                }
            }
        }

        h1 {
            font-size: 64px;
            font-style: italic;
            font-weight: 800;
            line-height: 130%;
            /* 83.2px */
            text-transform: uppercase;
            font-family: var(--geo);
            color: #fff;
            max-width: 939px;
            margin: 120px 0 0;

            @media (max-width: 1024px) {
                font-size: 20px;
                max-width: 298px;
                margin: 80px 0 0;
            }
        }
    }
}
</style>