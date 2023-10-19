<template>
    <div class="page">
        <h1>Магазин</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" v-model="search" @input="searchProducts" class="form-control"
                    placeholder="Поиск по товарам" aria-label="Поиск по товарам" aria-describedby="basic-addon1">
            </div>

            <div class="buttons">
                <button @click.stop="toggleFilter()">Фильтр</button>
                <button @click.stop="toggleSort()">Сортировка</button>
            </div>
            <div class="sort__body" :class="{ activeFil: sort }">
                <h2 @click="sortActive = 1, sortBy('price')" :class="{ activeh: sortActive == 1 }">Сначала дешевле</h2>
                <h2 @click="sortActive = 2, sortBy('-price')" :class="{ activeh: sortActive == 2 }">Сначала дороже</h2>
                <h2 @click="sortActive = 3, sortBy('rating')" :class="{ activeh: sortActive == 3 }">Полулярное</h2>
                <h2 @click="sortActive = 4, sortBy('-rating')" :class="{ activeh: sortActive == 4 }">Новое</h2>
            </div>
            <div class="filter__body" @click="stopPropagation" :class="{ activeFil: filter }">
                <div class="w-100">
                    <h2>Класс:</h2>
                    <div class="rare">
                        <div class="rare__gap">
                            <label class="custom-checkbox" v-for="item in filters['Класс']" :key="item">
                                <input type="checkbox" v-model="selectedRarity" :value="item" @change="applyFilters">
                                <p class="checkbox-text m-0">{{ item }}</p>
                            </label>
                        </div>

                    </div>
                </div>
                <div class="types">
                    <div class="select">
                        <h2>Качество:</h2>
                        <select name="" id="" v-model="selectedQual" @change="applyFilters">
                            <option value="" disabled selected>Все</option>
                            <option v-for="item in filters['Качество']" :key="item">{{ item }}</option>
                        </select>
                    </div>
                    <div class="select">
                        <h2>Тип</h2>
                        <select v-model="selectedType" @change="applyFilters">
                            <option v-for="item in filters['Тип']" :key="item">{{ item }}</option>
                        </select>
                    </div>
                    <div>
                        <h2>Цена</h2>
                        <div class="price">
                            <input type="number" id="from" name="from" placeholder="от" v-model="minPrice"
                                @input="applyFilters">
                            <img src="@/assets/img/line.svg" alt="">
                            <input type="number" id="to" name="to" placeholder="до" v-model="maxPrice"
                                @input="applyFilters">
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="catalog">
            <div class="catalog__items">
                <NuxtLink v-for="item in catalog.results" :key="item.id" :to="'/product/' + item.id" class="catalog__item">
                    <small>-{{ Math.floor(100 - (item.sell_price / item.price * 100)) }}%</small>
                    <img :src="item.img" alt="" class="item__image">
                    <p>{{ item.name }}</p>
                    <div class="price">
                        <small>{{ item.price.toFixed(1).toLocaleString() }} ₸</small>
                        <span>{{ item.sell_price.toFixed(1).toLocaleString() }} ₸</span>
                    </div>
                </NuxtLink>
            </div>
            <div class="text-center">
                <button ref="showmore" @click="loadMoreProducts">Показать еще</button>
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
            filter: false,
            sort: false,
            sortActive: 0,
            catalog: [],
            pathUrl: 'https://tf2shop.kz',
            kzt: null,
            usd: null,
            search: '',
            filters: [],
            selectedRarity: [],
            selectedQual: '',
            selectedType: '',
            minPrice: null,
            maxPrice: null,
        }
    },
    methods: {
        loadMoreProducts() {
            if (this.catalog.links.next) {
                this.$refs.showmore.innerHTML = 'Загружаем'
                axios
                    .get(this.catalog.links.next)
                    .then(response => {
                        this.$refs.showmore.innerHTML = 'Показать еще'
                        this.catalog.results.push(...response.data.results);
                        this.catalog.links.next = response.data.links.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        getCatalog() {
            const url = `${this.pathUrl}/api/products/catalog`

            axios
                .get(url)
                .then(response => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
        searchProducts() {
            const query = this.search.trim();
            if (query) {
                const queryParams = `?search=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getCatalog();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch(error => {
                    console.error(error);
                });
        },
        applyFilters() {
            let queryParams = '';
            if (this.selectedRarity) {
                queryParams += `&tags__класс=${this.selectedRarity}`;
            }
            if (this.selectedQual) {
                queryParams += `&tags__качество=${this.selectedHero}`;
            }
            if (this.selectedType) {
                queryParams += `&tags__тип=${this.selectedCell}`;
            }
            if (this.minPrice !== null) {
                queryParams += `&min_price=${this.minPrice}`;
            }
            if (this.maxPrice !== null) {
                queryParams += `&max_price=${this.maxPrice}`;
            }

            // Remove the leading '&' if there are any query parameters
            if (queryParams) {
                queryParams = `?${queryParams.substring(1)}`;
            }

            const path = `${this.pathUrl}/api/products/catalog${queryParams}`;
            axios
                .get(path)
                .then((response) => {
                    this.catalog = response.data;
                    this.filters = response.data.info_add.filter_type
                })
                .catch((error) => {
                    console.error(error);
                });
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
        sortBy(ordering) {
            this.sort = false
            const path = `${this.pathUrl}/api/products/catalog?ordering=${ordering}`;
            axios
                .get(path)
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
    title: 'Магазин | TF2Shop',
    ogTitle: 'Магазин | TF2Shop',
    description: 'Магазин | TF2Shop',
    ogDescription: 'Магазин | TF2Shop',
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
            width: 670px;
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
                    width: 100%;

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
                width: 100%;



                label {
                    display: block;
                    margin: 0;
                }

                .rare__gap {
                    display: grid;
                    gap: 20px;
                    grid-auto-flow: dense;
                    grid-template-columns: repeat(auto-fill, minmax(128px, 1fr));
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