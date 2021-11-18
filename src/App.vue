<template>
  <div id="app">
    <SiteHeader />
    <SiteMain />

    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import SiteMain from "./components/SiteMain.vue";
import SiteHeader from "./components/SiteHeader.vue";

export default {
  name: "App",
  components: {
    SiteHeader,
    SiteMain,
  },
};
</script>

<style lang="scss">
@import "./assets/scss/common.scss";
</style>
