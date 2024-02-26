<template>
    <Transition-group
        name="errorToBottom"
        tag="ul"
        class="errors"
        @after-enter="handleEnter"
        @before-leave="handleLeave"
    >
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
    methods: {
        handleEnter(el) {
            el.classList.add('errors__error--active');
            // console.log(el);
        },
        handleLeave(el) {
            el.classList.remove('errors__error--active');
            // console.log(el);
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.errors {
    padding: 0;

    &__error {
        color: $errorColor;
        display: grid;
        & > * {
            overflow: hidden;
        }
    }
}

.errorToBottom-enter-active {
    transition: transform $transitionSlow ease-in-out $transitionMedium,
        opacity $transitionMedium ease-in-out $transitionSlow,
        grid-template-rows $transitionSlow ease-in-out;
}
.errorToBottom-leave-active {
    transition: transform $transitionSlow ease-in-out,
        opacity $transitionMedium ease-in-out,
        grid-template-rows $transitionSlow ease-in-out $transitionSlow;
}
.errorToBottom-enter-to,
.errorToBottom-leave {
    grid-template-rows: 1fr;
    opacity: 1;
}
.errorToBottom-enter,
.errorToBottom-leave-to {
    opacity: 0;
    transform: translateY(-3rem);
    grid-template-rows: 0fr;
}
</style>
