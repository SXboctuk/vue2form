<template>
    <div class="patient-info">
        <div class="patient-info__content">
            <h3 class="patient-info__title">Базовая информация</h3>
            <div class="patient-info__item">
                <span class="patient-info__label">Фамилия: </span>
                <span class="patient-info__value">{{
                    baseInfo.lastName ?? 'Не указана'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Имя: </span>
                <span class="patient-info__value">{{
                    baseInfo.firstName ?? 'Не указано'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Отчество: </span>
                <span class="patient-info__value">{{
                    baseInfo.middleName ?? 'Не указано'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">День рождения: </span>
                <span class="patient-info__value">{{
                    baseInfo.birthDate ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Номер телефона: </span>
                <span class="patient-info__value">{{
                    baseInfo.phoneNumber ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Группа клиента: </span>
                <span class="patient-info__value">{{
                    baseInfo.clientTypes ?? 'Не указана'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Лечащий врач: </span>
                <span class="patient-info__value">{{
                    baseInfo.Therapist ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Не отправлять смс?: </span>
                <span class="patient-info__value">{{
                    baseInfo.dontSendSMS ? 'Да' : 'Нет'
                }}</span>
            </div>
        </div>
        <div class="patient-info__content">
            <h3 class="patient-info__title">Адрес</h3>
            <div class="patient-info__item">
                <span class="patient-info__label">Индекс: </span>
                <span class="patient-info__value">{{
                    address.index ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Страна: </span>
                <span class="patient-info__value">{{
                    address.country ?? 'Не указана'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Регион: </span>
                <span class="patient-info__value">{{
                    address.region ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Город: </span>
                <span class="patient-info__value">{{
                    address.city ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Улица: </span>
                <span class="patient-info__value">{{
                    address.street ?? 'Не указана'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Дом: </span>
                <span class="patient-info__value">{{
                    address.house ?? 'Не указан'
                }}</span>
            </div>
        </div>
        <div class="patient-info__content">
            <h3 class="patient-info__title">Документ идентификации</h3>
            <div class="patient-info__item">
                <span class="patient-info__label">Тип документа: </span>
                <span class="patient-info__value">{{
                    identityDocument.documentType ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Серия: </span>
                <span class="patient-info__value">{{
                    identityDocument.documentSeries ?? 'Не указана'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Номер: </span>
                <span class="patient-info__value">{{
                    identityDocument.documentNumber ?? 'Не указан'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Кем выдан: </span>
                <span class="patient-info__value">{{
                    identityDocument.issuedBy ?? 'Не указано'
                }}</span>
            </div>
            <div class="patient-info__item">
                <span class="patient-info__label">Дата выдачи: </span>
                <span class="patient-info__value">{{
                    identityDocument.issueDate ?? 'Не указана'
                }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AppPatientInfoShow',
    props: {
        baseInfo: {
            type: Object,
            required: true,
        },
        address: {
            type: Object,
            required: true,
        },
        identityDocument: {
            type: Object,
            required: true,
        },
    },
};
</script>

<style lang="scss" scoped>
@import '@/assets/variables.scss';
.patient-info {
    display: flex;
    flex-direction: column;
    gap: $gapY;
    &__content {
        display: flex;
        flex-direction: column;
        gap: $gapYSmall;
    }
    &__title {
        font-weight: bold;
        font-size: $titleSize;
    }
    &__item {
    }
    &__label {
        font-weight: bold;
        font-size: $textSize;
    }
    &__value {
    }
}
</style>
