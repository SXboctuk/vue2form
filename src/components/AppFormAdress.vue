<template>
    <app-form title="Адрес:">
        <app-input-text v-model="index" :label="'Индекс'" />
        <app-input-text v-model="country" :label="'Страна'" />
        <app-input-text v-model="region" :label="'Область'" />

        <app-input-with-errors :errors="v$.city.$errors">
            <app-input-text v-model="city" :label="'Город*'" />
        </app-input-with-errors>
        <app-input-text v-model="street" :label="'Улица'" />
        <app-input-text v-model="house" :label="'Дом'" />
        <slot :submit="submitForm" :pending="v$.$pending" />
    </app-form>
</template>

<script>
import AppInputText from './AppInputText.vue';
import AppForm from './AppForm.vue';
import AppInputWithErrors from './AppInputWithErrors.vue';

import { useVuelidate } from '@vuelidate/core';
import { required, helpers } from '@vuelidate/validators';

export default {
    name: 'AppFormPassport',
    components: {
        AppInputText,
        AppForm,
        AppInputWithErrors,
    },
    props: {
        formData: {
            type: Object,
            required: true,
        },
    },
    emits: ['submit', 'isValid'],
    model: {
        prop: 'formData',
        event: 'submit',
    },
    setup: () => {
        return { v$: useVuelidate() };
    },
    data: () => {
        return {
            index: null,
            country: null,
            region: null,
            city: null,
            street: null,
            house: null,
        };
    },
    created() {
        if (this.formData) {
            this.index = this.formData.index;
            this.country = this.formData.country;
            this.region = this.formData.region;
            this.city = this.formData.city;
            this.street = this.formData.street;
            this.house = this.formData.house;
        }
    },
    computed: {
        isFormValid() {
            return !this.v$.$invalid;
        },
    },
    methods: {
        async submitForm() {
            if (this.isFormValid) {
                this.$emit('submit', this.$data);
                return true;
            } else {
                await this.v$.$validate();
                if (this.isFormValid) {
                    return true;
                } else {
                    return false;
                }
            }
        },
    },
    watch: {
        isFormValid(value) {
            this.$emit('isValid', value);
            if (value) {
                this.$emit('submit', this.$data);
            }
        },
    },
    validations() {
        return {
            city: {
                required: helpers.withMessage(
                    'Поле обязательно для заполнения',
                    required
                ),
                minLength: helpers.withMessage(
                    'Минимальная длина 3 символа',
                    (value) => value.length >= 3
                ),
            },
        };
    },
};
</script>

<style scoped></style>
