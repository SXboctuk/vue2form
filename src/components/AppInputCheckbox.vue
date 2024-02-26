<template>
    <div class="input-checkbox">
        <label class="input-checkbox__label" v-if="label">{{ label }}</label>
        <div class="input-checkbox__wrapper">
            <input
                class="input-checkbox__input"
                :class="{ 'input-checkbox__input--active': modelValue }"
                type="checkbox"
                :value="modelValue"
                :checked="modelValue"
                @change="handleChange()"
            />
            <div class="input-checkbox__icon"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AppInputCheckbox',
    data: () => {
        return {};
    },
    methods: {
        handleChange() {
            this.$emit('update:model-value', !this.modelValue);
        },
    },
    props: {
        modelValue: {
            required: true,
            validator: (value) => {
                const vType = typeof value;
                return vType === 'boolean' || vType === 'null';
            },
        },
        label: {
            type: String,
            required: false,
        },
    },
    emits: ['update:model-value'],
    model: {
        prop: 'modelValue',
        event: 'update:model-value',
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-checkbox {
    width: fit-content;

    display: flex;
    align-items: center;
    justify-content: center;
    &__label {
        z-index: 5;
        height: 100%;
        padding: $labelInputPadding;
        font-size: $textSize;
        font-weight: bold;
        text-align: start;
    }
    &__wrapper {
        position: relative;

        &:has(.input-checkbox__input--active) {
            .input-checkbox__icon {
                opacity: 0.7;
            }
        }
    }
    &__icon {
        pointer-events: none;
        position: absolute;
        z-index: 100;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 1.2rem;
        height: 1.2rem;
        background-color: black;
        border-radius: $borderRadiusSmall;
        opacity: 0;

        transition: opacity $transitionSlow ease-in-out;
    }
    &__input {
        padding: $paddingInputWrapper;

        outline: none;

        font: inherit;
        color: currentColor;
        appearance: none;
        background-color: transparent;

        border-radius: $borderRadius;

        width: 2rem;
        height: 1.8rem;

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
        &--active {
            &::after {
                box-shadow: $afterBoxShadowActive;
            }
            &::before {
                box-shadow: $beforeBoxShadowActive;
            }
        }
    }
}
</style>
