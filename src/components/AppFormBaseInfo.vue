<template>
    <app-form title="Базовая информация:">
        <app-input-with-errors :errors="v$.lastName.$errors">
            <app-input-text
                v-model="lastName"
                :placeholder="'Введите фамилию'"
                :label="'Фамилия*'"
            />
        </app-input-with-errors>
        <app-input-with-errors :errors="v$.firstName.$errors">
            <app-input-text
                v-model="firstName"
                :placeholder="'Введите имя'"
                :label="'Имя*'"
            />
        </app-input-with-errors>

        <app-input-text
            v-model="middleName"
            :placeholder="'Введите отчество'"
            :label="'Отчество'"
        />
        <app-input-with-errors :errors="v$.birthDate.$errors">
            <app-input-date v-model="birthDate" :label="'Дата рождения*'" />
        </app-input-with-errors>

        <app-input-with-errors :errors="v$.phoneNumber.$errors">
            <app-input-phone-number
                v-model="phoneNumber"
                :label="'Номер телефона*'"
                :mask="'+7 (###) ###-##-##'"
            />
        </app-input-with-errors>
        <app-input-select
            v-model="sex"
            :label="'Пол'"
            :list="['Мужской', 'Женский']"
        />
        <app-input-with-errors :errors="v$.clientTypes.$errors">
            <app-input-multi-select
                v-model="clientTypes"
                :label="'Группа клиентов*'"
                :list="['VIP', 'Проблемные', 'ОМС']"
            />
        </app-input-with-errors>

        <app-input-select
            v-model="Therapist"
            :label="'Лечащий врач'"
            :type="'select'"
            :list="['Иванов', 'Захаров', 'Чернышева']"
        />
        <app-input-checkbox
            v-model="dontSendSMS"
            :label="'Не отправлять СМС'"
        />
        <slot :submit="submitForm" :pending="v$.$pending" />
    </app-form>
</template>

<script>
import AppInputText from './AppInputText.vue';
import AppInputPhoneNumber from './AppInputPhoneNumber.vue';
import AppInputSelect from './AppInputSelect.vue';
import AppInputMultiSelect from './AppInputMultiSelect.vue';
import AppInputDate from './AppInputDate.vue';
import AppInputCheckbox from './AppInputCheckbox.vue';
import AppForm from './AppForm.vue';
import AppInputWithErrors from './AppInputWithErrors.vue';

import { useVuelidate } from '@vuelidate/core';
import { required, helpers } from '@vuelidate/validators';

export default {
    name: 'AppBaseInfo',
    components: {
        AppForm,
        AppInputText,
        AppInputSelect,
        AppInputDate,
        AppInputCheckbox,
        AppInputPhoneNumber,
        AppInputMultiSelect,
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
            lastName: null,
            firstName: null,
            middleName: null,
            birthDate: null,
            phoneNumber: null,
            sex: null,
            clientTypes: null,
            Therapist: null,
            dontSendSMS: false,
        };
    },
    created() {
        if (this.formData) {
            this.lastName = this.formData.lastName;
            this.firstName = this.formData.firstName;
            this.middleName = this.formData.middleName;
            this.birthDate = this.formData.birthDate;
            this.phoneNumber = this.formData.phoneNumber;
            this.sex = this.formData.sex;
            this.clientTypes = this.formData.clientTypes;
            this.therapist = this.formData.therapist;
            this.dontSendSMS = this.formData.dontSendSMS;
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
            lastName: {
                required: helpers.withMessage(
                    'Поле обязательно для заполнения',
                    required
                ),
                minLength: helpers.withMessage(
                    'Минимальная длина 3 символа',
                    (value) => value.length >= 3
                ),
            },
            firstName: {
                required: helpers.withMessage(
                    'Поле обязательно для заполнения',
                    required
                ),
                minLength: helpers.withMessage(
                    'Минимальная длина 2 имени',
                    (value) => value.length >= 2
                ),
            },
            birthDate: {
                required: helpers.withMessage(
                    'Введите дату рождения',
                    required
                ),
            },
            phoneNumber: {
                required: helpers.withMessage(
                    'Введите номер телефона',
                    required
                ),
            },
            clientTypes: {
                required: helpers.withMessage(
                    'Выберите группы клиента',
                    required
                ),
            },
        };
    },
};
</script>

<style scoped></style>
