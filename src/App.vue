<template>
    <div class="container">
        <div class="container_components">
            <StepOne v-if="currentStep === 1" :current-image="getCurrentImage()"
                :tshirt-image="getShirtImage(printLocation)" :print-location="printLocation"
                @update:printLocation="printLocation = $event" />
            <StepTwo v-if="currentStep === 2" :current-index="currentIndex" :images="images" :print-location="printLocation"
                :current-image="getCurrentImage()" :tshirt-image="getShirtImage(printLocation)"
                @previous-image="previousImage" @next-image="nextImage" @random-image="getRandomImage" />
            <StepThree v-if="currentStep === 3" ref="stepThree" :inputs="inputsMock" :order-data="getOrderData()"
                @form-valid="handleFormValid" @next-step="nextStep" />
            <StepFour v-if="currentStep === 4" :shirt-image="getShirtImage(printLocation)"
                :current-image="getCurrentImage()" :inputs="getInputsData()" :total-price="calculateCost()" />
            <StepFive v-if="currentStep === 5" :order-data="getOrderData()" :total-price="calculateCost()"
                :current-image="getCurrentImage()" :tshirt-image="getShirtImage(printLocation)" />
        </div>

        <div v-if="currentStep !== 5" :class="{ 'navigation__summary': currentStep === 4 }" class="navigation">
            <button :disabled="currentStep === 1" :class="{ 'navigation__customStep': currentStep === 3 }"
                class="navigation__button" @click="previousStep">Wstecz</button>
            <button v-if="currentStep === 4" class="navigation__order__button" @click="placeOrder">Złóż zamówienie</button>
            <button v-else-if="currentStep !== 3" :disabled="currentStep === 4" class="navigation__button"
                @click="nextStep">Dalej</button>
        </div>

    </div>
</template>
  
<script>
import StepOne from './components/StepOne.vue';
import StepTwo from './components/StepTwo.vue';
import StepThree from './components/StepThree.vue';
import StepFour from './components/StepFour.vue';
import StepFive from './components/StepFive.vue';
import frontTshirt from './assets/icons/front.png';
import backTshirt from './assets/icons/back.png';

export default {
    components: { StepOne, StepTwo, StepThree, StepFour, StepFive },
    data() {
        return {
            currentStep: 1,
            printLocation: "front",
            currentIndex: 0,
            images: [],
            inputsMock: [],
            frontTshirtPic: frontTshirt,
            backTshirtPic: backTshirt,
            isStepThreeFormValid: false,
            isNextStepInProgress: false
        };
    },

    mounted() {
        this.fetchImages();
        this.generateInputsData();
    },

    methods: {
        nextStep() {
            if (this.isNextStepInProgress) {
                return;
            }

            if (this.currentStep === 3) {
                const isFormValid = this.$refs.stepThree.isFormValid();
                if (!isFormValid) {
                    return;
                }
            }
            this.isNextStepInProgress = true;

            this.$nextTick(() => {
                this.currentStep++;
                this.isNextStepInProgress = false;
            });
        },

        previousStep() {
            this.currentStep--;
        },

        handleFormValid(isValid) {
            this.isStepThreeFormValid = isValid;
        },
        async fetchImages() {
            try {
                for (let i = 0; i < 20; i++) {
                    const response = await fetch("https://picsum.photos/200/200");
                    this.images.push(response.url);
                }
            } catch (error) {
                console.error("Błąd podczas pobierania obrazka.", error);
            }
        },

        getCurrentImage() {
            return this.images[this.currentIndex];
        },

        previousImage() {
            if (this.currentIndex > 0) {
                this.currentIndex--;
            }
        },

        nextImage() {
            if (this.currentIndex < this.images.length - 1) {
                this.currentIndex++;
            }
        },

        getShirtImage() {
            if (this.printLocation === 'front') {
                return this.frontTshirtPic;
            } else if (this.printLocation === 'back') {
                return this.backTshirtPic;
            } else {
                return '';
            }
        },

        getRandomImage() {
            const randomIndex = Math.floor(Math.random() * this.images.length);
            this.currentIndex = randomIndex;
        },

        generateInputsData() {
            this.inputsMock = [
                { id: 'firstName', value: '', label: 'Imię', valid: true, error: false, errorMessage: 'Pole imię jest puste!' },
                { id: 'lastName', value: '', label: 'Nazwisko', valid: true, error: false, errorMessage: 'Pole nazwisko jest puste!' },
                { id: 'street', value: '', label: 'Ulica', valid: true, error: false, errorMessage: 'Pole Ulica jest puste!' },
                { id: 'buildingNumber', value: '', label: 'Numer budynku', valid: true, error: false, errorMessage: 'Pole numer budynku jest puste!' },
                { id: 'apartmentNumber', value: '', label: 'Numer mieszkania', valid: true, required: false, error: false, errorMessage: 'Pole numer mieszkania jest puste!' },
                { id: 'postalCode', value: '', label: 'Kod pocztowy', valid: true, error: false, errorMessage: 'Pole kod pocztowy jest puste!' },
                { id: 'city', value: '', label: 'Miasto', valid: true, error: false, errorMessage: 'Pole miasto jest puste!' },
                { id: 'email', value: '', label: 'Email', valid: true, error: false, errorMessage: 'Pole e-mail jest puste!' },
                { id: 'phoneNumber', value: '', label: 'Numer telefonu', valid: true, error: false, errorMessage: 'Pole numer telefonu jest puste!' }
            ];
        },

        getInputsData() {
            return this.inputsMock.map(input => ({ id: input.id, value: input.value, label: input.label }));
        },

        placeOrder() {
            this.currentStep++;
        },

        calculateCost() {
            return 10;
        },

        getOrderData() {
            const formData = this.inputsMock.reduce((data, input) => {
                data[input.id] = input.value;
                return data;
            }, {});

            return {
                printLocation: this.printLocation,
                selectedImage: this.images[this.currentIndex],
                totalCost: this.calculateCost(),
                ...formData,
            };
        },
    },
};
</script>
  
<style scoped lang="scss">
@import './assets/variables.scss';

.container {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100vh;

    .navigation {
        position: relative;
        top: -50px;
        display: flex;
        gap: 20px;

        &__order__button {
            background: $color-green;
            padding: 10px;
            color: $color-white;
            font-weight: bold;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
        }

        &__button {
            width: 80px;
            background: $color-orange;
            padding: 10px;
            color: $color-white;
            font-weight: bold;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
        }
    }

    .navigation__summary {
        position: relative;
        left: 115px;
    }

    .navigation__customStep {
        position: relative;
        top: -80px;
    }

}
</style>
  