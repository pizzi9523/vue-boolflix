<template>
  <header>
    <Logo />
    <div class="searchBox">
      <input v-model="searchText" type="search" name="" id="" />
      <button @click="$emit('search-movies', searchText)">Cerca</button>
    </div>
  </header>
</template>

<script>
import Logo from "./Logo.vue";
export default {
  data() {
    return {
      searchText: "",
    };
  },
  components: {
    Logo,
  },
};
</script>

<style lang="scss">
header {
  display: flex;
  justify-content: space-between;
  background-color: black;
  .searchBox {
    text-align: center;
    padding: 20px;
  }
}
</style>