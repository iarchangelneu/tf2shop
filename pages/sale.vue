<template>
    <div class="page">
        <h1>Инвентарь</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" :disabled="!sales" class="form-control" placeholder="Поиск по товарам"
                    aria-label="Поиск по товарам" aria-describedby="basic-addon1" v-model="search" @input="searchProducts">
            </div>

            <div class="buttons">
                <button @click="getTrans(), sales = false" v-if="sales">Покупки</button>
                <button @click="getInventory(), sales = true" v-if="!sales">Инвентарь</button>
                <button @click="getSteam()" :disabled="!sales" ref="update">Обновить инвентарь</button>
            </div>
        </div>
        <div class="empty" v-if="nado.length <= 0">
            <img src="@/assets/img/empty.png" class="img-fluid" alt="">
            <h1>инвентарь пока пуст</h1>
        </div>
        <div class="catalog" v-else>
            <div class="catalog__items">
                <div class="catalog__item" v-if="sales" v-for="item in inventory.results" :key="item.id">
                    <img :src="item.img" alt="" class="item__image">
                    <p>{{ item.name }}</p>
                    <div class="price">
                        <div>
                            <div class="sitee" @click="openModal(item.id)">
                                <img src="@/assets/img/site.svg" alt="">
                                <img src="@/assets/img/onsite.svg" class="onsite" alt="">
                            </div>

                        </div>
                        <span>{{ item.price.toFixed(1).toLocaleString() }} ₸</span>
                    </div>
                </div>
                <div class="catalog__item" v-if="!sales" v-for="item in inventory" :key="item.id">
                    <img :src="item.img" alt="" class="item__image">
                    <p>{{ item.name }}</p>
                    <div class="price">
                        <div>
                            <div class="sitee" @click="openModal2(item.id)">
                                <img src="@/assets/img/site.svg" alt="">
                                <img src="@/assets/img/onsite.svg" class="onsite" alt="">
                            </div>
                            <div class="steamee" @click="openSteam2(item.id)">
                                <img src="@/assets/img/steam.svg" alt="">
                                <img src="@/assets/img/insteam.svg" class="insteam" alt="">
                            </div>

                        </div>
                        <span>{{ item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                    </div>
                </div>

            </div>
            <div class="text-center" v-if="sales">
                <button ref="showmore" @click="loadMoreProducts">Показать еще</button>
            </div>
        </div>

    </div>
    <ModalFirst :product="modal"></ModalFirst>
    <SteamModal :product="modal"></SteamModal>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            filter: false,
            sort: false,
            sortActive: 0,
            pathUrl: 'https://tf2shop.kz',
            inventory: [],
            kzt: null,
            usd: null,
            modal: {},
            sales: true,
            search: '',
            nado: [],
        }
    },
    methods: {
        loadMoreProducts() {
            if (this.inventory.links.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.inventory.links.next)
                    .then(response => {
                        this.$refs.showmore.innerHTML = 'Показать еще'
                        this.inventory.results.push(...response.data.results);
                        this.inventory.links.next = response.data.links.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        openModal(id) {
            this.modal = this.inventory.results.filter(item => item.id == id)[0];
            $('#modalFirst').modal('show')

        },
        openSteam(id) {
            this.modal = this.inventory.results.filter(item => item.id == id)[0];
            $('#steamModal').modal('show')
        },
        openModal2(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#modalFirst').modal('show')

        },
        openSteam2(id) {
            this.modal = this.inventory.filter(item => item.id == id)[0];
            $('#steamModal').modal('show')
        },
        getTrans() {
            const url = `${this.pathUrl}/api/users/profile/`;
            const token = this.getAuthorizationCookie();
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res);
                    this.inventory = res.data.transactions_item.map(item => item.item);
                    this.nado = res.data.transactions_item.map(item => item.item);
                });
        },
        getSteam() {
            const url = `${this.pathUrl}/api/products/update-inventory`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            this.$refs.update.innerHTML = 'Загружаем'

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.getInventory()
                    this.$refs.update.innerHTML = 'Обновить инвентарь'

                })
                .catch((error) => {
                    console.log(error)
                })
        },
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getInventory();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/get-inventory${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.inventory = response.data
                })
                .catch(error => {
                    console.error(error);
                });
        },

        getInventory() {
            const url = `${this.pathUrl}/api/products/get-inventory`

            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(url)
                .then((res) => {
                    console.log(res)
                    this.inventory = res.data
                    this.nado = res.data.results
                })
        },
        toggleFilter() {
            this.filter = !this.filter;
            this.sort = false
            if (this.filter) {
                document.addEventListener('click', this.closeFilter);
            } else {
                document.removeEventListener('click', this.closeFilter);
            }
        },
        closeFilter() {
            this.filter = false;
            document.removeEventListener('click', this.closeFilter);
        },
        toggleSort() {
            this.sort = !this.sort;
            this.filter = false
            if (this.sort) {
                document.addEventListener('click', this.closeSort);
            } else {
                document.removeEventListener('click', this.closeSort);
            }
        },
        closeSort() {
            this.sort = false;
            document.removeEventListener('click', this.closeSort);
        },
        stopPropagation(event) {
            event.stopPropagation();
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')

        if (accType == 'steam' || accType == 'email') {
            this.getInventory()
        }
        else {
            this.$router.push('/register')

        }
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Инвентарь | TF2Shop',
    ogTitle: 'Инвентарь | TF2Shop',
    description: 'Инвентарь | TF2Shop',
    ogDescription: 'Инвентарь | TF2Shop',
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

    .empty {
        margin-top: 60px;
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        gap: 20px;

        h1 {
            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            /* 26px */
            text-transform: uppercase;
            font-family: var(--geo);
            color: #fff;
            text-align: center;

            @media (max-width: 1024px) {
                font-size: 16px;
            }
        }
    }

    .catalog {
        margin-top: 30px;

        button {
            margin-top: 20px;
            border-radius: 10px;
            background: #FF1D00;
            border: 0;
            padding: 13px 15px;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: normal;
            font-family: var(--geo);
            color: #fff;
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
                    justify-content: space-between;

                    div {
                        display: flex;
                        gap: 30px;
                        position: relative;

                        img {
                            cursor: pointer;
                        }

                        .sitee:hover .onsite {
                            transform: scale(1) !important;
                        }

                        .steamee:hover .insteam {
                            transform: scale(1) !important;
                        }

                        .onsite,
                        .insteam {
                            position: absolute;
                            transform: scale(0);
                            transition: all .3s ease;
                        }

                        .onsite {
                            top: -150%;
                            left: 0;
                        }

                        .insteam {
                            top: -150%;
                            left: 0;
                        }

                    }

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

    .filters {
        display: flex;
        gap: 30px;
        position: relative;
        align-items: center;

        @media (max-width: 1024px) {
            gap: 20px;
            flex-direction: column;
        }

        .activeFil {
            transform: scale(1) !important;
        }

        .sort__body {
            position: absolute;
            left: 42.8%;
            top: 0;
            transform: scale(0);
            transition: all .3s ease;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #0500FF;
            background: #292854;
            padding: 20px;
            text-align: center;
            z-index: 5;


            @media (max-width: 1024px) {
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .activeh {
                border-radius: 5px;
                background: #181831;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--geo);
                color: #fff;
                margin: 0 0 12px;
                cursor: pointer;
                padding: 6px 12px;
                transition: all .3s ease;

                &:last-child {
                    margin: 0;
                }
            }
        }

        .filter__body {
            position: absolute;
            transform: scale(0);
            transition: all .3s ease;
            left: 19.2%;
            top: 0;
            margin-top: 70px;
            border-radius: 10px;
            border: 1px solid #0500FF;
            background: #292854;
            padding: 15px 20px;
            display: flex;
            z-index: 5;
            gap: 71px;

            @media (max-width: 1024px) {
                flex-direction: column;
                gap: 10px;
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            .price {
                display: flex;
                gap: 4px;
                align-items: center;

                input {
                    border-radius: 5px;
                    border: 1px solid #0500FF;
                    background: #181831;
                    padding: 3px 5px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--geo);
                    color: #fff;
                    max-width: 130px;
                    height: 30px;


                    &::placeholder {
                        color: #fff;
                    }
                }
            }

            .types {
                .select {
                    margin: 0 0 30px;
                }

                h2 {
                    margin: 0 0 10px !important;
                }

                select {
                    border-radius: 5px;
                    border: 1px solid #0500FF;
                    background: #181831;
                    color: #fff;
                    padding: 5px 10px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--geo);
                    width: 14.583vw;

                    @media (max-width: 1024px) {
                        width: 100%;
                    }
                }
            }

            h2 {

                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--geo);
                margin: 0 0 20px;
                color: #fff;

                &:last-child {
                    margin: 0 0 10px;
                }
            }

            .rare {
                display: flex;
                gap: 30px;

                @media (max-width: 1024px) {
                    gap: 36px;
                }

                label {
                    display: block;
                }

                div {
                    p {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--geo);
                        color: #fff;
                    }
                }
            }

        }

        .input-group {
            max-width: 620px;

            @media (max-width: 1024px) {
                max-width: 100%;
            }
        }

        .input-group-text {
            border-radius: 10px;
            border: 2px solid #FF1D00;
            background: #2A1310;
            border-right: 0;
            border-top-right-radius: 0;
            border-bottom-right-radius: 0;
        }

        input {
            border-radius: 10px;
            border: 2px solid #FF1D00;
            background: #2A1310;
            border-left: 0;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
            padding: 10px 20px 10px 0;
            box-shadow: none;

            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: 32px;
            font-family: var(--geo);
            color: #fff;

            &::placeholder {
                color: #fff;
            }
        }

        .buttons {
            display: flex;
            gap: 30px;
            align-items: center;

            @media (max-width: 1024px) {
                gap: 20px;
                width: 100%;
                flex-direction: column;
                align-items: normal;

            }

            button {
                border-radius: 10px;
                background: var(--Blue, #0500FF);
                padding: 10px 30px;
                border: 0;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {
                    flex: 1;
                    padding: 10px 0;
                    font-size: 16px;
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

.custom-checkbox p,
.custom-checkbox a {
    @media (max-width: 1024px) {
        font-size: 12px !important;
    }
}

.custom-checkbox input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    border-radius: 3px;
}

.custom-checkbox .checkbox-text:before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 10px;
    border: 2px solid #C8C8C8;
    border-radius: 3px;
    margin-bottom: 3px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/check.svg');
    font-size: 10px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: transparent;
}

.custom-checkbox {
    margin-bottom: 20px;

}

.custom-checkbox:last-child {
    margin-bottom: 0 !important;

    @media (max-width: 1024px) {
        margin-bottom: 22px !important;
    }
}
</style>