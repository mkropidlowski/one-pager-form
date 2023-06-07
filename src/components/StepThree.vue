<template>
    <div class="container">
        <h2 class="container__heading">Dane zamawiającego</h2>
        <form class="container__form" @submit.prevent="submitForm">
            <div v-for="input in inputs" :key="input.id">
                <input v-model="input.value" :type="input.type" :placeholder="input.label" class="container__form_input"
                    required :class="{ 'container__form_invalid': !input.valid }" @input="validateInput(input)" />
                <div v-if="!input.valid" class="container__form_error-message">{{ input.errorMessage }}</div>
            </div>
        </form>
    </div>
</template>
  
<script>
export default {
    props: {
        inputs: {
            type: Array,
            required: true,
        }
    },
    emits: ['form-valid'],
    methods: {
        validateInput(input) {
            input.valid = !!input.value;
        },
        isFormValid() {
            return this.inputs.every(input => input.valid);
        },
        submitForm() {
            this.inputs.forEach(input => {
                this.validateInput(input);
            });

            const isFormValid = this.isFormValid();
            this.$emit('form-valid', isFormValid);
        },
    }
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

        &_error-message {
            margin-bottom: 10px;
            font-weight: bold;
            color: red;
            text-align: center;
        }

        &_submit-button {
            margin-top: 20px;
            padding: 8px 16px;
            background-color: $color-green;
            color: $color-white;
            font-weight: bold;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    }
}
</style>
  