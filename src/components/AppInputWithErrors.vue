<template>
    <div>
        <slot />
        <template>
            <app-input-errors :errors="errors" />
        </template>
    </div>
</template>

<script>
import AppInputErrors from './AppInputErrors.vue';
export default {
    name: 'AppInputWithErrors',
    components: {
        AppInputErrors,
    },
    props: {
        errors: {
            type: Array,
            default: () => [],
        },
    },
};
</script>

<style lang="scss" scoped></style>
