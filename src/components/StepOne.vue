<template>
    <div class="container">
        <h2 class="container__heading">Wybierz miejsce nadruku</h2>
        <div class="container__radioButtons">
            <label>
                <input v-model="selectedLocation" type="radio" value="front" /> Przód
            </label>
            <label>
                <input v-model="selectedLocation" type="radio" value="back" /> Tył
            </label>
        </div>
        <div class="container__image">
            <img :src="tshirtImage" alt="T-shirt" width="350px" height="350px" class="container__image_tshirt-Type" />
            <img :src="currentImage" alt="Wybrane zdjęcie" class="container__image_tshirt-Chosen" />
        </div>
    </div>
</template>
  
<script>

import frontTshirt from '../assets/icons/front.png';
import backTshirt from '../assets/icons/back.png';

export default {

    props: {
        // eslint-disable-next-line vue/require-default-prop
        currentImage: {
            type: String,
            requried: true
        },
        // eslint-disable-next-line vue/require-default-prop
        printLocation: {
            type: String,
            requried: true
        }
    },
    emits: ['update:printLocation'],
    data() {
        return {
            selectedLocation: this.printLocation,
            frontTshirtPic: frontTshirt,
            backTshirtPic: backTshirt
        };
    },
    computed: {
        // eslint-disable-next-line vue/no-dupe-keys
        tshirtImage() {
            if (this.selectedLocation === 'front') {
                return this.frontTshirtPic;
            } else if (this.selectedLocation === 'back') {
                return this.backTshirtPic;
            } else {
                return '';
            }
        }
    },
    watch: {
        selectedLocation(newLocation) {
            this.$emit('update:printLocation', newLocation);
        }
    }
};
</script>

<style scoped lang="scss">
@import '../assets/variables.scss';

.container {
    max-width: 900px;
    height: fit-content;
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

    &__radioButtons {
        background: $color-light-blue;
        padding: 15px;
        border-radius: 5px;
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