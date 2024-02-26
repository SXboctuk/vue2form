<template>
    <div
        class="input-text"
        :class="{
            'input-text--active': modelValue && modelValue.length > 0,
        }"
    >
        <label class="input-text__label" v-if="label">{{ label }}</label>
        <input
            class="input-text__input"
            type="'text'"
            :placeholder="placeholder ?? ''"
            :value="modelValue"
            @input="handleInput($event.target.value)"
        />
    </div>
</template>

<script>
export default {
    name: 'AppInputText',

    props: {
        modelValue: {
            required: true,
            validator: (value) => {
                return typeof value === 'string' || value === null;
            },
        },
        placeholder: {
            type: String,
            required: false,
        },
        label: {
            type: String,
            required: false,
        },
        errors: {
            type: Array,
            required: false,
        },
    },

    emits: ['update:model-value'],
    model: {
        prop: 'modelValue',
        event: 'update:model-value',
    },
    methods: {
        handleInput(value) {
            const data = value.trim().length > 0 ? value.trim() : null;
            this.$emit('update:model-value', data);
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-text {
    position: relative;

    border-radius: $borderRadius;
    display: flex;
    align-items: center;
    justify-content: center;

    padding: $paddingXY;
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
    &:has(&__input:focus) {
        &::after {
            box-shadow: $afterBoxShadow;
        }
        &::before {
            box-shadow: $beforeBoxShadow;
        }
    }

    &__input {
        z-index: 2;
        border-radius: 0 $borderRadius $borderRadius 0;
        font-size: $textSize;
        border: none;
        outline: none;
        width: 100%;
    }

    &__label {
        z-index: 5;
        height: 100%;
        padding: $labelInputPadding;
        font-size: $textSize;
        font-weight: bold;
        text-align: start;
    }
}
</style>
