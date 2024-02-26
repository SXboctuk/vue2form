<template>
    <div
        class="input-phone"
        :class="{ 'input-phone--active': modelValue ? true : false }"
    >
        <label class="input-phone__label" v-if="label">{{ label }}</label>
        <input
            class="input-phone__input"
            ref="input"
            type="'text'"
            :value="formatted"
            @keydown="validateInput($event)"
            @input="formatInput($event)"
            @click="handleClick($event)"
        />
    </div>
</template>

<script>
export default {
    name: 'AppInputPhoneNumber',

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
        mask: {
            type: String,
            required: true,
        },
    },
    emits: ['update:model-value'],
    data: () => {
        return {
            inputValue: '',
        };
    },
    methods: {
        validateInput(event) {
            if (!/\D+/g.test(event.key)) {
                if (this.inputValue.length >= this.maskAvailableLenght) {
                    event.preventDefault();
                }
                return;
            }
            if (event.key === 'Backspace') {
                if (this.inputValue.length === 0) {
                    event.preventDefault();
                    return;
                }
                this.inputValue = this.inputValue.slice(
                    0,
                    this.inputValue.length - 1
                );
            }

            event.preventDefault();
        },
        formatInput(event) {
            this.formatted = event.target.value;
        },
        handleClick(event) {
            event.preventDefault();
            this.cursorToStart(event.target);
        },
        cursorToStart(target) {
            let start = target.value.indexOf('#');

            if (start == -1) {
                start = target.value.length;
            }

            target.setSelectionRange(start, start);
        },
        clearMask(str) {
            const clearNum = str.slice(this.maskStart).replace(/[^0-9.]/g, '');

            return clearNum;
        },
        masking(str) {
            let result = [...this.mask];
            if (str.length === 0) {
                return this.mask;
            }
            for (let i = 0; i < str.length; i++) {
                const index = result.indexOf('#');
                result[index] = str[i];
            }
            return result.join('');
        },
    },
    computed: {
        formatted: {
            get() {
                return this.masking(this.inputValue);
            },
            set(newVal) {
                this.inputValue = this.clearMask(newVal);
            },
        },
        maskAvailableLenght() {
            return this.mask.replace(/[^#]+/g, '').length;
        },
        maskStart() {
            return this.mask.indexOf('#');
        },
        maskNumbers() {
            return this.mask.replace(/[^0-9.]/g, '');
        },
    },
    watch: {
        formatted: function () {
            this.$nextTick(() => {
                this.cursorToStart(this.$refs.input);
            });
        },
        inputValue: function (val) {
            if (val.length === this.maskAvailableLenght) {
                this.$emit('update:model-value', this.maskNumbers + val);
            } else {
                this.$emit('update:model-value', null);
            }
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.input-phone {
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
    }
}
</style>
