<template>
    <div
        class="input-multi-select"
        :class="{
            'input-multi-select--active': modelValue && modelValue.length > 0,
        }"
    >
        <label class="input-multi-select__label" v-if="label">{{
            label
        }}</label>
        <div
            class="input-multi-select__select"
            :value="modelValue"
            @change="handleSelect($event.target.value)"
        >
            <div
                class="input-multi-select__option"
                :class="{
                    'input-multi-select__option--active':
                        modelValue && modelValue.includes(item),
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
            if (this.modelValue === null) {
                this.$emit('update:model-value', [value]);
            } else {
                const index = this.modelValue.indexOf(value);
                let dataToEmit = this.modelValue;

                if (index !== -1) {
                    dataToEmit = dataToEmit.toSpliced(index, 1);
                    if (dataToEmit.length === 0) {
                        dataToEmit = null;
                    }
                } else {
                    dataToEmit.push(value);
                }
                this.$emit('update:model-value', dataToEmit);
            }
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-multi-select {
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
    &__select {
        display: flex;
        gap: 2rem;
        width: 100%;
    }
    &__option {
        position: relative;
        padding: $paddingInputWrapper;
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
