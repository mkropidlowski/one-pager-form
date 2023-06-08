<template>
    <div class="container">
        <h2 class="container__heading">Dane zamawiającego</h2>
        <form class="container__form" @submit.prevent="submitForm">
            <div v-for="input in inputs" :key="input.id" class="container__form_inputBox">
                <label :for="input.id" class="container__form_label" v-show="input.valid">
                    {{ input.label }}
                </label>
                <label :for="input.id" class="container__form_error-message" v-show="!input.valid">
                    {{ input.errorMessage }}
                </label>
                <input v-model="input.value" :type="input.type" class="container__form_input" required
                    :class="{ 'container__form_invalid': !input.valid }" @input="validateInput(input)" />
            </div>

        </form>
        <button class="container__form_button" type="submit" :disabled="!isFormValid()" @click="submitForm">Dalej</button>
    </div>
</template>
  
<script>
export default {
    props: {
        inputs: {
            type: Array,
            required: true,
        },
    },
    emits: ['form-valid', 'next-step'],
    methods: {
        validateInput(input) {
            input.valid = !!input.value;
        },
        isFormValid() {
            return this.inputs.every((input) => input.valid);
        },
        submitForm() {
            this.inputs.forEach((input) => {
                this.validateInput(input);
            });

            const isFormValid = this.isFormValid();
            this.$emit('form-valid', isFormValid);

            if (isFormValid) {
                this.$emit('next-step');
            }
        },
    },
};
</script>
  
<style scoped lang="scss">
@import '../assets/variables.scss';

.container {
    margin-top: 20px;

    &__heading {
        background: $color-green;
        padding: 10px;
        font-size: 18px;
        border-radius: 8px;
        color: $color-white;
        font-weight: bold;
        text-align: center;
        margin-bottom: 40px;
    }

    &__form {
        display: flex;
        justify-content: center;
        flex-flow: wrap;
        gap: 20px;
        width: 900px;
        text-align: center;

        &_inputBox {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }

        &_label {
            font-size: 13px;
            margin-bottom: 5px;
            font-weight: bold;
            color: #000;
            text-align: center;
        }

        &_error-message {
            font-size: 13px;
            margin-bottom: 5px;
            font-weight: bold;
            color: red;
            text-align: center;
        }

        &_input {
            width: 250px;
            padding: 8px;
            margin-bottom: 10px;
            border: 2px solid black;
            border-radius: 5px;
        }

        &_invalid {
            border: 2px solid red;
        }

        &_button {
            width: 80px;
            margin-top: 30px;
            background-color: $color-green;
            padding: 10px;
            color: $color-white;
            font-weight: bold;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
        }
    }
}
</style>
  
<style scoped lang="scss">