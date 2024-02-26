<template>
    <button
        class="button"
        @click="$emit('click')"
        :type="type"
        :disabled="disabled"
    >
        <slot />
    </button>
</template>

<script>
export default {
    name: 'AppButton',
    emits: ['click'],
    props: {
        type: {
            type: String,
            default: 'button',
        },
        disabled: {
            type: Boolean,
            default: false,
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.button {
    border: none;
    outline: none;

    width: fit-content;
    position: relative;
    border-radius: $borderRadius;

    padding: $paddingXY;

    font-weight: bold;
    font-size: $textSize;
    color: white;
    background-color: gray;

    transition: transform $transitionFast ease-in-out,
        opacity $transitionFast ease-in-out;

    &:hover {
        transform: scale(1.1);
    }
    &:disabled {
        opacity: 0.5;
        pointer-events: none;
    }
    &::after {
        content: '';
        position: absolute;
        z-index: 3;
        inset: 0;

        border-radius: $borderRadius;
        box-shadow: $afterBoxShadow;

        transition: box-shadow $transitionFast ease-in-out;
    }

    &:active {
        &::after {
            box-shadow: $afterBoxShadowActive;
        }
    }
}
</style>
