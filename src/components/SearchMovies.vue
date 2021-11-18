<template>
  <div class="searchBox">
    <input v-model="searchText" type="search" name="" id="" />
    <button @click="$emit('search-movies', searchText)">Cerca</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchText: "",
    };
  },
};
</script>

<style lang="scss">
.searchBox {
  text-align: center;
}
</style>