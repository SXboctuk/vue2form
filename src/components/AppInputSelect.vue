<template>
    <div
        class="input-select"
        :class="{
            'input-select--active': modelValue && modelValue.length > 0,
        }"
    >
        <label class="input-select__label" v-if="label">{{ label }}</label>
        <div
            class="input-select__select"
            :value="modelValue"
            @change="handleSelect($event.target.value)"
        >
            <div
                class="input-select__option"
                :class="{
                    'input-select__option--active': modelValue === item,
                }"
                v-for="item in list"
                :key="item"
                :value="item"
                @click="handleOption(item)"
            >
                {{ item }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AppInputSelect',

    props: {
        modelValue: {
            required: true,
            validator: (value) => {
                if (Array.isArray(value)) {
                    return value.every((item) => typeof item === 'string');
                } else {
                    return typeof value === 'string' || value === null;
                }
            },
        },
        label: {
            type: String,
            required: false,
        },
        list: {
            type: Array,
            required: true,
            validator: (value) => {
                if (Array.isArray(value)) {
                    return value.every((item) => typeof item === 'string');
                } else {
                    return false;
                }
            },
        },
    },
    emits: ['update:model-value'],
    model: {
        prop: 'modelValue',
        event: 'update:model-value',
    },
    methods: {
        handleOption(value) {
            if (this.modelValue === value) {
                this.$emit('update:model-value', null);
            } else {
                this.$emit('update:model-value', value);
            }
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-select {
    width: fit-content;

    border-radius: $borderRadius;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-inline: $paddingX;

    &__label {
        z-index: 5;
        height: 100%;
        padding: $labelInputPadding;
        font-size: $textSize;
        font-weight: bold;
        text-align: start;
    }
    &__select {
        display: flex;
        flex-wrap: wrap;
        column-gap: $gapX;
        row-gap: $gapY;
        width: 100%;
    }
    &__option {
        position: relative;
        padding: $paddingXY;
        cursor: pointer;
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
