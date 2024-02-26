<template>
    <div class="form-patient">
        <nav class="form-patient__nav">
            <li
                :class="{ 'form-patient__nav-item--active': step === 1 }"
                class="form-patient__nav-item"
                @click="step = 1"
            >
                Базовая информация
            </li>
            <li
                :class="{ 'form-patient__nav-item--active': step === 2 }"
                class="form-patient__nav-item"
                @click="step = 2"
            >
                Адрес
            </li>
            <li
                :class="{ 'form-patient__nav-item--active': step === 3 }"
                class="form-patient__nav-item"
                @click="step = 3"
            >
                Паспортные данные
            </li>
        </nav>
        <div v-show="step === 1">
            <keep-alive max="5">
                <app-form-base-info
                    v-slot="{ pending }"
                    v-model="baseInfo"
                    @isValid="(value) => (baseInfoStatus = value)"
                    ref="baseInfoRef"
                >
                    <div class="form-patient__buttons">
                        <app-button :disabled="true">Назад</app-button>
                        <app-button
                            @click="toNext($refs.baseInfoRef.submitForm, 1)"
                            :disabled="pending"
                            >Далее</app-button
                        >
                    </div>
                </app-form-base-info>
            </keep-alive>
        </div>
        <div v-show="step === 2">
            <app-form-adress
                v-model="adress"
                v-slot="{ pending }"
                @isValid="(value) => (adressStatus = value)"
                ref="adressRef"
            >
                <div class="form-patient__buttons">
                    <app-button @click="toPrev(2)" :disabled="false"
                        >Назад</app-button
                    >
                    <app-button
                        @click="toNext($refs.adressRef.submitForm, 2)"
                        :disabled="pending"
                        >Далее</app-button
                    >
                </div>
            </app-form-adress>
        </div>
        <div v-show="step === 3">
            <app-form-identity-document
                v-model="identityDocument"
                v-slot="{ pending }"
                @isValid="(value) => (identityDocumentStatus = value)"
                ref="identityDocumentRef"
            >
                <div class="form-patient__buttons">
                    <app-button @click="toPrev(3)" :disabled="false"
                        >Назад</app-button
                    >
                    <app-button
                        @click="toNext($refs.identityDocumentRef.submitForm, 3)"
                        :disabled="pending || pendingSubmitForm"
                        >Отправить</app-button
                    >
                </div>
            </app-form-identity-document>
        </div>
        <template v-if="step === -1"
            ><app-patient-info-show
                :base-info="baseInfo"
                :address="adress"
                :identity-document="identityDocument"
        /></template>
    </div>
</template>

<script>
import AppFormBaseInfo from '@/components/AppFormBaseInfo.vue';
import AppFormAdress from '@/components/AppFormAdress.vue';
import AppFormIdentityDocument from '@/components/AppFormIdentityDocument.vue';
import AppPatientInfoShow from './AppPatientInfoShow.vue';
import AppButton from '@/components/AppButton.vue';

const FORM_LENGHT = 3;
export default {
    name: 'AppFormPatient',
    components: {
        AppFormBaseInfo,
        AppFormAdress,
        AppFormIdentityDocument,
        AppPatientInfoShow,
        AppButton,
    },
    data: () => {
        return {
            step: 1,
            baseInfoStatus: false,
            baseInfo: {
                lastName: null,
                firstName: null,
                middleName: null,
                birthDate: null,
                phoneNumber: null,
                sex: null,
                clientTypes: null,
                Therapist: null,
                dontSendSMS: false,
            },
            adressStatus: false,
            adress: {
                index: null,
                country: null,
                region: null,
                city: null,
                street: null,
                house: null,
            },
            identityDocumentStatus: false,
            identityDocument: {
                documentType: null,
                documentSeries: null,
                documentNumber: null,
                issuedBy: null,
                issueDate: null,
            },

            pendingSubmitForm: false,
        };
    },
    methods: {
        async toNext(submit, num) {
            const result = await submit();
            if (result) {
                if (num === FORM_LENGHT) {
                    const result = await this.submitForm();
                    if (result) {
                        this.step = -1;
                        return;
                    }
                    return;
                }
                this.step = num + 1;
            }
        },
        toPrev(num) {
            this.step = num - 1;
        },
        async submitForm() {
            this.pendingSubmitForm = true;
            if (!this.identityDocumentStatus) {
                await this.$refs.identityDocumentRef.submitForm();
                this.step = 3;

                this.pendingSubmitForm = false;
                return false;
            }
            if (!this.adressStatus) {
                await this.$refs.adressRef.submitForm();
                this.step = 2;

                this.pendingSubmitForm = false;
                return false;
            }
            if (!this.baseInfoStatus) {
                await this.$refs.baseInfoRef.submitForm();
                this.step = 1;

                this.pendingSubmitForm = false;
                return false;
            }

            return true;
        },
    },
};
</script>

<style lang="scss" scoped>
@import '@/assets/normalize.css';
@import '@/assets/variables.scss';

.form-patient {
    display: flex;
    flex-direction: column;
    justify-content: center;
    max-width: 60rem;
    padding: $paddingXY;
    margin: $marginY auto;

    position: relative;
    &::after {
        content: '';
        position: absolute;
        z-index: 3;
        inset: 0;

        border-radius: $borderRadius;
        box-shadow: $afterBoxShadow;

        transition: box-shadow $transitionSlow ease-in-out;
    }
    &::before {
        content: '';
        position: absolute;
        z-index: 1;
        inset: 0;

        border-radius: $borderRadius;
        box-shadow: $beforeBoxShadow;

        transition: box-shadow $transitionSlow ease-in-out;
    }
    &__buttons {
        display: flex;
        justify-content: space-between;
    }
    &__nav {
        display: flex;
        justify-content: space-between;
        margin-block: $marginY;
    }
    &__nav-item {
        cursor: pointer;
        list-style-type: none;
        font-size: $textSize;

        &--active {
            font-weight: bold;
        }
    }
}
</style>
