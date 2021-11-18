<template>
  <div id="app">
    <SiteMain />

    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import SiteMain from "./components/SiteMain.vue";

export default {
  name: "App",
  components: {
    SiteMain,
  },
};
</script>

<style lang="scss">
@import "./assets/scss/common.scss";
</style>
