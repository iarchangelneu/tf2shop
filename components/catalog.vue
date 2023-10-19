<template>
    <div class="page">
        <h1>магазин</h1>

        <div class="filters">
            <div class="input-group">
                <div class="input-group-prepend">
                    <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/search.svg" alt=""></span>
                </div>
                <input type="text" class="form-control" placeholder="Поиск по товарам" aria-describedby="basic-addon1">
            </div>
            <div class="buttons">
                <button @click.stop="toggleFilter">фильтр</button>
                <button @click.stop="toggleSort">сортировка</button>
            </div>
            <div class="sort__body" :class="{ activeFil: sort }">
                <h2>Сначала дешевле</h2>
                <h2>Сначала дороже</h2>
                <h2>Полулярное</h2>
                <h2>Новое</h2>
            </div>
            <div class="filter__body" @click="stopPropagation" :class="{ activeFil: filters }">
                <div>
                    <h2>Редкость:</h2>
                    <div class="rare">
                        <div>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Rare
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Common
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Uncommon
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Mythical
                                </p>
                            </label>
                        </div>
                        <div>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Legendary
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Arcana
                                </p>
                            </label>
                            <label class="custom-checkbox">
                                <input type="checkbox">
                                <p class="checkbox-text m-0">Immortal
                                </p>
                            </label>

                        </div>
                    </div>
                </div>
                <div class="types">
                    <div class="select">
                        <h2>Герой</h2>
                        <select name="" id="">
                            <option value="" disabled selected>Любой</option>
                        </select>
                    </div>
                    <div class="select">
                        <h2>Ячейка</h2>
                        <select name="" id="">
                            <option value="" disabled selected>Любая</option>
                        </select>
                    </div>
                    <div>
                        <h2>Цена</h2>
                        <div class="price">
                            <input type="number" id="from" name="from" placeholder="от">
                            <img src="@/assets/img/line.svg" alt="">
                            <input type="number" id="to" name="to" placeholder="до">
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="catalog__body">
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
            <NuxtLink :to="'/product/1'" class="catalog__item green">
                <img src="@/assets/img/catalog1.png" alt="">
                <p>Fiery Soul of the Slayer</p>
                <div class="text-right">
                    <span>19 800 ₸</span>
                </div>
            </NuxtLink>
        </div>

        <div class="text-center">
            <button ref="showmore">Показать еще</button>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            sort: false,
            filters: false,
        }
    },
    methods: {
        toggleFilter() {
            this.filters = !this.filters;
            if (this.filters) {
                document.addEventListener('click', this.closeFilter);
            } else {
                document.removeEventListener('click', this.closeFilter);
            }
        },
        closeFilter() {
            this.filters = false;
            document.removeEventListener('click', this.closeFilter);
        },
        toggleSort() {
            this.sort = !this.sort;
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
    }
}
</script>
<script setup>
useSeoMeta({
    title: 'Каталог | DotaShop',
    ogTitle: 'Каталог | DotaShop',
    description: 'Каталог | DotaShop',
    ogDescription: 'Каталог | DotaShop',
})
</script>
<style lang="scss" scoped>
.page {
    padding: 73px 70px 110px 200px;

    @media (max-width: 1600px) {
        padding: 73px 50px 110px 10.417vw;
    }

    @media (max-width: 1024px) {
        padding: 150px 20px 50px;
    }

    .text-center {
        margin-top: 40px;

        button {
            padding: 10px 47px;
            border-radius: 10px;
            border: 1px solid var(--iris-100, #5D5FEF);
            background: var(--iris-100, #5D5FEF);

            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--mon);
            color: #fff;
        }
    }

    .catalog__body {
        display: grid;
        gap: 30px;
        grid-auto-flow: dense;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
        margin-top: 30px;

        @media (max-width: 1024px) {
            gap: 20px;
            margin-top: 20px;
            grid-template-columns: repeat(auto-fill, minmax(100%, 1fr));
        }

        @media (max-width: 500px) {
            grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
        }

        .catalog__item {
            text-decoration: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
            border-radius: 10px;
            background: linear-gradient(90deg, #24212B 0%, rgba(36, 33, 43, 0.65) 100%);
            padding: 10px;

            img {
                width: 100%;
                height: 154px;
                object-fit: cover;

                @media (max-width: 1024px) {
                    height: 97px;
                }
            }

            p,
            span {
                margin: 0;
                font-size: 16px;
                font-style: normal;
                font-weight: 600;
                line-height: 130%;
                font-family: var(--mon);
                color: #fff;
            }
        }

        .green {
            border: 2px solid #ADE55C;
        }

        .golden {
            border: 2px solid #E4AE39;
        }

        .blackblue {
            border: 2px solid #3348E0;
        }

        .whiteblue {
            border: 2px solid #458AD9;
        }

        .calblue {
            border: 2px solid #B0C3D9;
        }

        .purple {
            border: 2px solid #7047F2;
        }
    }

    .input-group-text {
        border-radius: 10px;
        background-color: #24212B;
        padding: 0 20px;
        border: 0;
        border-right: 0;
        border-top-right-radius: 0;
        border-bottom-right-radius: 0;
    }

    .filters {
        margin-top: 30px;
        display: flex;
        gap: 20px;
        position: relative;

        @media (max-width: 1024px) {
            flex-direction: column;
        }

        .buttons {
            display: flex;
            gap: 20px;
        }

        .activeFil {
            transform: scale(1) !important;
        }

        .sort__body {
            position: absolute;
            left: 50.5%;
            top: 0;
            transform: scale(0);
            transition: all .3s ease;
            margin-top: 50px;
            border-radius: 10px;
            padding: 20px;
            background: #292238;

            @media (max-width: 1024px) {
                width: 100%;
                left: 0;
                margin-top: 125px;
            }

            h2 {
                font-size: 16px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                text-transform: uppercase;
                font-family: var(--mon);
                color: #fff;
                margin: 0 0 20px;

                &:last-child {
                    margin: 0;
                }
            }
        }

        .filter__body {
            position: absolute;
            transform: scale(0);
            transition: all .3s ease;
            left: 10%;
            top: 0;
            margin-top: 50px;
            border-radius: 10px;
            padding: 15px 20px;
            background: #292238;
            display: flex;
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
                    background: #24212B;
                    padding: 3px 5px;
                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
                    color: #fff;
                    max-width: 100px;
                    border-radius: 0;
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
                    background: #24212B;
                    border: 0;
                    color: #fff;
                    padding: 5px 10px;

                    font-size: 16px;
                    font-style: normal;
                    font-weight: 500;
                    line-height: normal;
                    font-family: var(--mon);
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
                font-family: var(--mon);
                margin: 0 0 20px;
                color: #fff;

                &:last-child {
                    margin: 0 0 10px;
                }
            }

            .rare {
                display: flex;
                gap: 30px;

                label {
                    display: block;
                }

                div {
                    p {
                        font-size: 16px;
                        font-style: normal;
                        font-weight: 500;
                        line-height: normal;
                        font-family: var(--mon);
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

        button {
            flex: 1;
            max-width: 195px;
            padding: 10px 20px;
            border: 1px solid var(--iris-100, #5D5FEF);
            background: var(--iris-100, #5D5FEF);
            border-radius: 10px;

            font-size: 16px;
            font-style: normal;
            font-weight: 500;
            line-height: 130%;
            text-transform: uppercase;
            font-family: var(--mon);
            color: #fff;

            @media (max-width: 1024px) {
                max-width: 100%;
            }
        }

        input {
            border-radius: 10px;
            background: #24212B;
            padding: 10px 20px 10px 0;
            border-left: 0;
            border: 0;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;

            font-size: 20px;
            font-style: normal;
            font-weight: 400;
            line-height: 32px;
            font-family: var(--mon);
            color: #fff;
            box-shadow: none;



            &::placeholder {
                color: #fff;
            }


        }
    }

    h1 {
        font-size: 32px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        letter-spacing: 1.6px;
        text-transform: uppercase;
        font-family: var(--col);
        color: #FFF;
        margin: 0;
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