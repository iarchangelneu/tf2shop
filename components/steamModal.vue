<template>
    <div class="modal fade" id="steamModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <div class="text-center">
                        <h1>вывести предмет в steam</h1>
                        <p>Вы хотите вывести {{ product.name }} в Steam?</p>
                        <p>Инструкция по получению предмета будет отправлена на Вашу почту</p>

                        <button @click="steamOut(product.id)">ПОДТВЕРДИТЬ</button>
                    </div>
                </div>

            </div>
        </div>
    </div>
    <EmailModal></EmailModal>
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
            pathUrl: 'https://tf2shop.kz',
        }
    },
    methods: {
        steamOut(id) {
            const url = `${this.pathUrl}/api/users/send-email`
            const token = this.getAuthorizationCookie();

            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .post(url, {
                    id: id,
                })
                .then((res) => {
                    console.log(res)
                    if (res.status == 200) {
                        $('#steamModal').modal('hide')
                    }
                })
                .catch((error) => {
                    console.log(error)
                    $('#steamModal').modal('hide')
                    $('#emailModal').modal('show')
                })
        },
    }
}
</script>
<style lang="scss" scoped>
.modal-body {
    padding: 0;

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
        margin: 0 0 30px;

        @media (max-width: 1024px) {
            text-align: center;
            font-size: 16px;
            margin: 0 0 20px;
        }
    }

    button {
        border-radius: 10px;
        border: 0;
        background: var(--iris-100, #0500FF);
        padding: 10px 20px;

        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--geo);
        color: #fff;
    }

    p {
        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--geo);
        color: #fff;
        margin: 0 0 18px;
    }
}

.modal-content {
    border-radius: 10px;
    border: 1px solid var(--LightRed, #FF1D00);
    background: #2A1310;
    box-shadow: 0px 4px 56px 0px rgba(5, 0, 255, 0.50);
    padding: 30px;
}

.modal-dialog {
    max-width: 596px;
    margin: 1.75rem auto;
}
</style>