<template>
    <footer v-if="!hideFooterOnPages.includes($route.name)">

        <div class="footer__body">
            <img src="@/assets/img/footerlogo.svg" class="logo" alt="">

            <div>
                <div class="links">
                    <div>
                        <NuxtLink to="/catalog">Магазин</NuxtLink>
                        <NuxtLink to="/sale">Продажа</NuxtLink>
                    </div>
                    <div>
                        <a href="/#faq">FAQ</a>
                        <a href="/#sales">Акции</a>
                    </div>
                    <div>
                        <NuxtLink to="/polytics">Политика конфиденциальности</NuxtLink>
                        <NuxtLink to="/terms">Пользовательское соглашение</NuxtLink>
                    </div>
                </div>
                <div class="copyright">
                    <small>г. Алматы, пр-кт Республики 13, Почтовый индекс A15X3C5 (050013), +7 775 816 82 48, БИН
                        210840016943<br>
                        Почта поддержки : support@tf2shop.kz</small>
                    <small>2023©tf2shop.kz </small>
                </div>
            </div>
        </div>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            hideFooterOnPages: ['login', 'register'],
        }
    }
}
</script>
<style lang="scss" scoped>
footer {
    padding: 50px 6.667vw 50px 8.073vw;

    @media (max-width: 1600px) {
        padding: 40px 50px 50px 10.417vw;
    }

    @media (max-width: 1024px) {
        padding: 0 20px 40px;
    }

    img {
        @media (max-width: 1400px) {
            max-width: 20%;
        }

        @media (max-width: 1024px) {
            max-width: 100%;
        }
    }

    .logo {
        @media (max-width: 1024px) {
            width: 150px;
        }
    }

    .footer__body {
        margin-top: 50px;
        display: flex;
        align-items: center;
        gap: 6.458vw;

        @media (max-width: 1600px) {
            gap: 50px;
        }

        @media (max-width: 1024px) {
            flex-direction: column;
            align-items: flex-start;
            gap: 30px;
        }
    }

    .copyright {
        display: flex;
        gap: 20px;
        margin-top: 39px;

        @media (max-width: 1024px) {
            flex-direction: column;
            gap: 10px;
            margin-top: 20px;
        }

        small {
            font-size: 16px;
            font-style: normal;
            font-weight: 400;
            line-height: 130%;
            font-family: var(--geo);
            color: #C8C8C8;
        }
    }

    .links {
        display: flex;
        gap: 6.979vw;

        @media (max-width: 1600px) {
            gap: 50px;
        }

        @media (max-width: 1024px) {
            gap: 30px;
            flex-direction: column;
        }

        @media (max-width: 380px) {
            flex-direction: column;
        }

        div {
            display: flex;
            flex-direction: column;
            gap: 29px;

            a {
                font-size: 20px;
                font-style: normal;
                font-weight: 500;
                line-height: normal;
                // text-transform: uppercase;
                font-family: var(--geo);
                color: #fff;
                text-decoration: none;

                @media (max-width: 1600px) {
                    font-size: 20px;

                }

                @media (max-width: 1024px) {
                    font-size: 16px;

                }

                @media (max-width: 380px) {
                    font-size: 14px;
                }

            }
        }
    }

    hr {
        margin-top: 0;
        border-top: 2px solid #fff;
    }
}
</style>