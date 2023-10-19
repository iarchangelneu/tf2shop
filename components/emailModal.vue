<template>
    <div class="modal fade" id="emailModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">

                <div class="modal-body">
                    <div class="text-center">
                        <h1>Получить предмет</h1>
                        <p>Ошибка!</p>
                        <p>Чтобы получить предмет, необходимо привязать E-mail в Личном кабинете</p>

                        <NuxtLink to="/account">Личный кабинет</NuxtLink>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {}
    }
}
</script>

<style lang="scss" scoped>
.modal-body {
    padding: 0;

    h1 {
        font-size: 20px;
        font-style: normal;
        font-weight: 400;
        line-height: 130%;
        /* 31.2px */
        letter-spacing: 1.2px;
        text-transform: uppercase;
        font-family: var(--unb);
        color: #fff;
        margin: 0 0 30px;

        @media (max-width: 1024px) {
            text-align: center;
            font-size: 16px;
            margin: 0 0 20px;
        }
    }

    a {
        border-radius: 10px;
        border: 0;
        background: var(--iris-100, #452777);
        padding: 10px 20px;
        display: inline-block;
        text-decoration: none;

        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
    }

    p {
        font-size: 20px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--mon);
        color: #fff;
        margin: 0 0 18px;
    }
}

.modal-content {
    border-radius: 10px;
    background: var(--Bg2, #160E21);
    box-shadow: 0px 4px 16px 0px rgba(86, 31, 140, 0.30);
    padding: 20px 20px 50px;
}

.modal-dialog {
    max-width: 596px;
    margin: 1.75rem auto;
}
</style>