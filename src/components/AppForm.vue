<template>
    <form class="app-form" @submit.prevent="$emit('submit')">
        <h2 v-if="title" class="app-form__title">{{ title }}</h2>
        <slot />
    </form>
</template>

<script>
export default {
    name: 'AppForm',
    props: {
        title: {
            type: String,
            required: false,
        },
    },
    emits: {
        submit: {
            required: false,
        },
    },
};
</script>

<style lang="scss" scoped>
@import '../assets/variables.scss';
.app-form {
    max-width: 60rem;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 1.4rem;
    &__title {
        font-size: $titleSize;
    }
}
</style>
