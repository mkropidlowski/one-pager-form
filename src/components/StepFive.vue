<template>
    <div class="stepFiveBox__container">
        <div class="stepFiveBox__container_header">
            <h3 class="stepFiveBox__heading">Dziękujemy za złożenie zamówienia.</h3>
            <button class="stepFiveBox__reloadButton" @click="reloadPage">Zamów ponownie</button>
        </div>
        <div class="stepFiveBox__summary">
            <h2 class="stepFiveBox__summary__heading">Twoje dane: </h2>
            <p><span>Imię:</span> {{ orderData.firstName }}</p>
            <p><span>Nazwisko:</span> {{ orderData.lastName }}</p>
            <p><span>Ulica:</span> {{ orderData.street }}</p>
            <p><span>Numer budynku:</span> {{ orderData.buildingNumber }}</p>
            <p><span>Numer mieszkania:</span> {{ orderData.apartmentNumber }}</p>
            <p><span>Kod pocztowy:</span> {{ orderData.postalCode }}</p>
            <p><span>Miasto:</span> {{ orderData.city }}</p>
            <p><span>Telefon:</span> {{ orderData.phone }}</p>
            <p><span>Adres mailowy:</span> {{ orderData.email }}</p>
            <p class="stepFiveBox__summary__price"><span>Koszt zamówienia:</span> {{ totalPrice }} zł</p>
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

.stepFiveBox__container {
    &_header {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 30px;
        padding: 20px;
        border-radius: 10px;
        background: whitesmoke;
    }
}

.stepFiveBox {
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

    &__summary {
        margin-top: 50px;

        p {
            margin-top: 5px;

            span {
                font-weight: bold;
                padding: 3px;
            }
        }

        &__heading {
            font-weight: bold;
            text-decoration: underline;
            margin-bottom: 15px;
        }

        &__price {
            font-weight: bold;
            padding: 10px;
            color: $color-white;
            border-radius: 5px;
            background: $color-light-green;
        }
    }
}
</style>
  