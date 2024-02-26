<template>
    <div id="app">
        <AppFormBaseInfo />
    </div>
</template>

<script>
import AppFormBaseInfo from './components/AppFormBaseInfo.vue';

export default {
    name: 'App',
    components: {
        AppFormBaseInfo,
    },
};
</script>

<style lang="scss">
@import './assets/normalize.css';
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
}
html {
    font-size: 14px;
    @media screen and (min-width: 480px) {
        font-size: 16px;
    }
    @media screen and (min-width: 640px) {
        font-size: 17px;
    }
    @media screen and (min-width: 1024px) {
        font-size: 19px;
    }
}
</style>
