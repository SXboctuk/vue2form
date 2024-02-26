<template>
    <app-form title="Документ удостоверяющий личность:">
        <app-input-with-errors :errors="v$.documentType.$errors">
            <app-input-select
                v-model="documentType"
                :label="'Тип документа*.'"
                :list="[
                    'Паспорт',
                    'Свидетельство о рождении',
                    'Вод. удостоверение',
                ]"
            />
        </app-input-with-errors>
        <app-input-text v-model="documentSeries" :label="'Серия'" />
        <app-input-text v-model="documentNumber" :label="'Номер'" />
        <app-input-text v-model="issuedBy" :label="'Кем выдан'" />

        <app-input-with-errors :errors="v$.issueDate.$errors">
            <app-input-date v-model="issueDate" :label="'Дата выдачи*'" />
        </app-input-with-errors>
        <slot :submit="submitForm" :pending="v$.$pending" />
    </app-form>
</template>

<script>
import AppInputText from './AppInputText.vue';
import AppInputSelect from './AppInputSelect.vue';
import AppInputDate from './AppInputDate.vue';
import AppForm from './AppForm.vue';
import AppInputWithErrors from './AppInputWithErrors.vue';

import { useVuelidate } from '@vuelidate/core';
import { required, helpers } from '@vuelidate/validators';

export default {
    name: 'AppFormIdentityDocument',
    components: {
        AppForm,
        AppInputText,
        AppInputSelect,
        AppInputDate,
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
            documentType: null,
            documentSeries: null,
            documentNumber: null,
            issuedBy: null,
            issueDate: null,
        };
    },
    created() {
        if (this.formData) {
            this.documentType = this.formData.documentType;
            this.documentSeries = this.formData.documentSeries;
            this.documentNumber = this.formData.documentNumber;
            this.issuedBy = this.formData.issuedBy;
            this.issueDate = this.formData.issueDate;
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
            documentType: {
                required: helpers.withMessage(
                    'Поле обязательно для заполнения',
                    required
                ),
            },
            issueDate: {
                required: helpers.withMessage(
                    'Поле обязательно для заполнения',
                    required
                ),
            },
        };
    },
};
</script>

<style scoped></style>
