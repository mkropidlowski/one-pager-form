<template>
    <div class="container">
        <h2 class="container__heading">Wybierz zdjęcie</h2>
        <div class="container__buttons">
            <button :disabled="currentIndex === 0" class="button" @click="previousImage">Poprzednie zdjęcie</button>
            <button class="button" @click="getRandomImage">Losowe</button>
            <button :disabled="currentIndex === images.length - 1" class="button" @click="nextImage">Następne
                zdjęcie</button>
        </div>
        <div class="container__image">
            <img :src="tshirtImage" alt="T-shirt" width="350px" height="350px" class="container__image_tshirt-Type" />
            <img :src="currentImage" alt="Wybrane zdjęcie" class="container__image_tshirt-Chosen" />
        </div>
    </div>
</template>

<script>

export default {
    props: {
        currentIndex: {
            type: Number,
            required: true
        },
        images: {
            type: Array,
            required: true
        },
        tshirtImage: {
            type: String,
            required: true,
        },
        currentImage: {
            type: String,
            required: true
        }
    },
    emits: ['previous-image', 'next-image', 'random-image'],

    methods: {
        previousImage() {
            if (this.currentIndex > 0) {
                this.$emit('previous-image');
            }
        },
        nextImage() {
            if (this.currentIndex < this.images.length - 1) {
                this.$emit('next-image');
            }
        },
        getRandomImage() {
            this.$emit('random-image');
        },
    }
};
</script>
  
<style scoped lang="scss">
@import '../assets/variables.scss';

.container {
    display: flex;
    gap: 30px;
    flex-direction: column;
    align-items: center;

    &__heading {
        background: $color-green;
        padding: 10px;
        font-size: 18px;
        border-radius: 8px;
        color: $color-white;
        font-weight: bold;
    }

    &__buttons {
        display: flex;
        gap: 20px;

        .button {
            background: $color-blue;
            padding: 10px;
            color: $color-white;
            font-weight: bold;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
        }
    }

    &__image {
        display: flex;
        align-items: center;
        flex-direction: column;
        height: 380px;

        &_tshirt-Type {
            position: absolute;
            z-index: -1;
        }

        &_tshirt-Chosen {
            position: absolute;
            margin-top: 100px;
            width: 120px;
            height: 120px;
        }
    }
}
</style>