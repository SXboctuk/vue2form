<template>
    <div
        class="input-date"
        :class="{
            'input-date--active': modelValue ? true : false,
        }"
    >
        <label class="input-date__label" v-if="label">{{ label }}</label>
        <input
            class="input-date__input"
            type="date"
            :value="dateToInputFormat"
            @input="handleInput($event.target.value)"
        />
    </div>
</template>

<script>
export default {
    name: 'AppInputDate',

    props: {
        modelValue: {
            required: true,
            validator: (value) => {
                return value instanceof Date || value === null;
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
    computed: {
        dateToInputFormat: function () {
            if (this.modelValue instanceof Date) {
                const day = this.modelValue.getDate();
                const month = this.modelValue.getMonth() + 1;
                const year = this.modelValue.getFullYear();

                return `${year}-${month.toString().padStart(2, '0')}-${day
                    .toString()
                    .padStart(2, '0')}`;
            } else {
                return null;
            }
        },
    },
    methods: {
        handleInput(value) {
            if (value === '') {
                this.$emit('update:model-value', null);
            } else {
                this.$emit('update:model-value', new Date(value));
            }
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-date {
    width: fit-content;
    display: flex;
    align-items: center;
    justify-content: center;

    position: relative;

    padding: $paddingInputWrapper;

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
    &__label {
        z-index: 5;
        height: 100%;
        padding: $labelInputPadding;
        font-size: $textSize;
        font-weight: bold;
        text-align: start;
    }
    &__input {
        z-index: 2;
        border-radius: 0 $borderRadius $borderRadius 0;

        font-size: $textSize;
        border: none;
        outline: none;

        position: relative;

        &::-webkit-calendar-picker-indicator {
            bottom: 0;
            cursor: pointer;
            height: auto;
            left: 0;
            position: absolute;
            padding-left: 86%;
            top: 8%;
            // width: auto;
        }
    }
}
</style>
