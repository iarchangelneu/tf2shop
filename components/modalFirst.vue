<template>
    <div class="modal fade" id="modalFirst" tabindex="-1" aria-labelledby="modalFirst" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <h1>Продать</h1>

                    <div class="item__body">
                        <div class="img">
                            <img :src="product.img" alt="">
                        </div>


                        <div>
                            <h2>{{ product.name }}</h2>

                            <span>
                                Ваша цена:
                            </span>

                            <div class="price">
                                <input type="number" v-model="cost">
                                <button @click="saleItem(product.name, product.id)">ПРОДАТЬ</button>
                            </div>
                            <small>{{ error }}</small>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
    <ModalSecond :name="name"></ModalSecond>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    props: {
        product: Object,
    },
    data() {
        return {
            cost: null,
            error: '',
            pathUrl: 'https://tf2shop.kz',
            name: '',
        }
    },
    methods: {
        saleItem(name, id) {
            const url = `${this.pathUrl}/api/products/sale-item/${id}/${this.cost}`
            const token = this.getAuthorizationCookie();
            this.name = name
            if (this.cost <= 0) {
                this.error = 'Сумма должна быть > 0'
            }
            else {
                axios.defaults.headers.common['Authorization'] = `Token ${token}`;

                axios
                    .get(url)
                    .then((res) => {
                        console.log(res)
                        if (res.status == 200) {
                            $('#modalFirst').modal('hide')
                            $('#modalSecond').modal('show')
                        }
                    })
                    .catch((error) => {
                        console.log(error)
                    })
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.modal-body {
    padding: 0;

    .item__body {
        display: flex;
        align-items: flex-start;
        gap: 30px;

        @media (max-width: 1024px) {
            flex-direction: column;
            width: 100%;
        }

        .img {
            min-width: 180px;
            height: 154px;
            background: #000;

            img {
                width: 100%;
                height: 100%;
                object-fit: cover;
            }
        }

        small {
            font-family: var(--geo);
            color: red;
            font-size: 14px;
            margin: 10px 0;
        }

        .price {
            display: flex;
            gap: 10px;
            align-items: center;

            input {
                border-radius: 10px;
                border: 2px solid var(--LightRed, #FF1D00);
                background: transparent;
                max-width: 164px;
                padding: 13px 10px;

                font-size: 16px;
                font-style: normal;
                font-weight: 400;
                line-height: normal;
                font-family: var(--geo);
                color: #fff;

                @media (max-width: 1024px) {}
            }

            button {
                border-radius: 10px;
                border: 0;
                background: #0500FF;
                padding: 10px 20px;
                flex: 1;

                font-size: 20px;
                font-style: normal;
                font-weight: 400;
                line-height: 130%;
                font-family: var(--geo);
                color: #fff;
            }
        }

        span {
            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: normal;
            font-family: var(--geo);
            color: #fff;
            margin: 0 0 10px;
            display: block;
        }

        h2 {
            font-size: 20px;
            font-style: normal;
            font-weight: 300;
            line-height: 130%;
            font-family: var(--geo);
            color: #fff;
            margin: 0 0 15px;
        }

    }

    h1 {
        font-size: 32px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 31.2px */
        letter-spacing: 1.2px;
        text-transform: uppercase;
        font-family: var(--fun);
        color: #fff;
        margin: 0 0 20px;

        @media (max-width: 1024px) {
            font-size: 16px;
        }
    }
}

.modal-content {
    border-radius: 10px;
    border: 1px solid var(--LightRed, #FF1D00);
    background: #2A1310;
    box-shadow: 0px 4px 56px 0px rgba(5, 0, 255, 0.50);
    padding: 20px 30px 50px;
}

@media (min-width: 576px) {
    .modal-dialog {
        max-width: 596px;
        margin: 1.75rem auto;
    }
}

@media (max-width: 1024px) {
    .modal-dialog {
        max-width: 350px;
        margin: 1.75rem auto;
    }
}
</style>