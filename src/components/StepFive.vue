<template>
    <div class="container">
        <div class="container__header">
            <h3 class="container__heading">Dziękujemy za złożenie zamówienia.</h3>
            <button class="container__reloadButton" @click="reloadPage">Zamów ponownie</button>
        </div>
        <div class="container__summaryBox">
            <div class="container__summaryBox_image">
                <img :src="tshirtImage" alt="T-shirt" width="350px" height="350px" class="image_tshirt-Type" />
                <img :src="currentImage" alt="Wybrane zdjęcie" class="image_tshirt-Chosen" />
            </div>
            <div class="container__summaryBox__info">
                <h2 class="container__summaryBox__info-heading">Twoje dane: </h2>
                <p><span>Imię:</span> {{ orderData.firstName }}</p>
                <p><span>Nazwisko:</span> {{ orderData.lastName }}</p>
                <p><span>Ulica:</span> {{ orderData.street }}</p>
                <p><span>Numer budynku:</span> {{ orderData.buildingNumber }}</p>
                <p><span>Numer mieszkania:</span> {{ orderData.apartmentNumber }}</p>
                <p><span>Kod pocztowy:</span> {{ orderData.postalCode }}</p>
                <p><span>Miasto:</span> {{ orderData.city }}</p>
                <p><span>Telefon:</span> {{ orderData.phone }}</p>
                <p><span>Adres mailowy:</span> {{ orderData.email }}</p>
                <p class="container__summaryBox__info-price"><span>Koszt zamówienia:</span> {{ totalPrice }} zł</p>
            </div>
        </div>

    </div>
</template>
  
<script>

export default {
    props: {
        orderData: {
            type: Object,
            required: true
        },
        // eslint-disable-next-line vue/require-default-prop
        totalPrice: {
            type: Number,
        },
        // eslint-disable-next-line vue/require-default-prop
        tshirtImage: {
            type: String,
            required: true,
        },
        currentImage: {
            type: String,
            required: true
        }
    },
    mounted() {
        console.log(this.orderData);
    },
    methods: {
        reloadPage() {
            location.reload();
        }
    }
};
</script>

<style scoped lang="scss">
@import '../assets/variables.scss';

.container {
    &__header {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 30px;
        padding: 20px;
        border-radius: 10px;
        background: whitesmoke;
    }

    &__heading {
        font-weight: bold;
    }

    &__reloadButton {
        width: 200px;
        background: $color-blue;
        padding: 10px;
        color: $color-white;
        font-weight: bold;
        border: 0;
        border-radius: 10px;
        cursor: pointer;
    }

    &__summaryBox {
        width: 800px;
        margin-top: 50px;
        display: flex;
        align-items: center;
        justify-content: space-around;

        &_image {
            display: flex;
            align-items: center;
            flex-direction: column;
            height: 380px;

            .image_tshirt-Type {
                position: absolute;
                z-index: -1;
            }

            .image_tshirt-Chosen {
                position: absolute;
                margin-top: 100px;
                width: 120px;
                height: 120px;
            }

        }

        &__info {
            width: 340px;

            p {
                margin-top: 5px;

                span {
                    font-weight: bold;
                    padding: 3px;
                }
            }

            &-heading {
                font-weight: bold;
                text-decoration: underline;
                margin-bottom: 15px;
            }

            &-price {

                font-weight: bold;
                padding: 10px;
                color: $color-white;
                border-radius: 5px;
                background: $color-light-green;
            }
        }
    }


}
</style>
  