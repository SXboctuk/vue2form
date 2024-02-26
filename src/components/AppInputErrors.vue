<template>
    <Transition-group name="errorToBottom" tag="ul" class="errors">
        <li class="errors__error" v-for="error in errors" :key="error.$uid">
            <div>{{ error.$message }}</div>
        </li>
    </Transition-group>
</template>

<script>
export default {
    name: 'AppInputErrors',
    data: () => {
        return {};
    },
    props: {
        errors: {
            type: Array,
            default: () => [],
        },
    },
    methods: {},
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.errors {
    padding: 0;

    &__error {
        position: relative;
        z-index: -1;
        padding-inline: $paddingX;
        color: $errorColor;
        display: grid;
        margin-top: $paddingY;
        & > * {
            overflow: hidden;
        }
    }
}

.errorToBottom-enter-active {
    transition: transform $transitionSlow ease-in-out $transitionMedium,
        opacity $transitionMedium ease-in-out $transitionSlow,
        grid-template-rows $transitionSlow ease-in-out,
        margin $transitionFast ease-in-out;
}
.errorToBottom-leave-active {
    transition: transform $transitionSlow ease-in-out $transitionSlow,
        opacity $transitionMedium ease-in-out,
        grid-template-rows $transitionSlow ease-in-out $transitionSlow,
        margin $transitionFast ease-in-out $transitionSlow;
}
.errorToBottom-enter-to,
.errorToBottom-leave {
    grid-template-rows: 1fr;
    opacity: 1;
    margin-top: $paddingY;
}
.errorToBottom-enter,
.errorToBottom-leave-to {
    opacity: 0;
    transform: translateY(-3rem);
    grid-template-rows: 0fr;
    margin-top: 0;
}
</style>
