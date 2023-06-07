<template>
    <div class="container">
        <h2 class="container__heading">Podsumowanie i zamówienie</h2>
        <div class="container__summary">
            <div class="container__summary_tshirt">
                <div class="container__summary_tshirt-box">
                    <img :src="shirtImage" alt="T-shirt" width="350px" height="350px" class="tshirt-box-Image" />
                    <img :src="currentImage" alt="Wybrane zdjęcie" class="tshirt-box-Chose" />
                </div>
            </div>

            <div class="container__summary_userInfo">
                <h3 class="container__summary_userInfo-heading">Dane zamawiającego:</h3>
                <p v-for="(input, index) in inputs" :key="index">
                    <span>{{ input.label }}:</span> {{ input.value }}
                </p>
                <p class="summary__price">
                    <span>Koszt zamówienia:</span> {{ totalPrice }} zł
                </p>

            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    props: {
        // eslint-disable-next-line vue/require-default-prop
        totalPrice: {
            type: Number,
            required: true
        },
        // eslint-disable-next-line vue/require-default-prop
        inputs: {
            type: Array,
            required: true,
        },
        shirtImage: {
            type: String,
            required: true,
        },
        currentImage: {
            type: String,
            required: true
        }
    },
};
</script>

<style scoped lang="scss">
@import '../assets/variables.scss';

.container {
    min-width: 800px;

    &__heading {
        background: $color-green;
        padding: 10px;
        font-size: 18px;
        border-radius: 8px;
        color: $color-white;
        font-weight: bold;
    }

    &__summary {
        display: flex;
        justify-content: space-around;
        gap: 20px;
        margin-top: 20px;
        width: 100%;

        &_tshirt {
            &-box {
                display: flex;
                justify-content: center;

                .tshirt-box-Image {
                    position: absolute;
                    z-index: -1;
                }

                .tshirt-box-Chose {
                    position: absolute;
                    margin-top: 100px;
                    width: 120px;
                    height: 120px;
                }
            }
        }

        &_userInfo {
            &-heading {
                font-weight: bold;
                padding: 10px;
                text-decoration: underline;
            }

            p {
                margin-top: 5px;

                span {
                    font-weight: bold;
                    padding: 3px;
                }
            }

            .summary__price {
                font-weight: bold;
                padding: 10px;
                margin-top: 10px;
                color: $color-white;
                border-radius: 5px;
                background: $color-light-green;
            }
        }
    }

}
</style>
  