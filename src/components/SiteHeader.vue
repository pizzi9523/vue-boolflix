<template>
  <header>
    <Logo />
    <div class="searchBox">
      <input
        placeholder="Search"
        v-model="searchText"
        type="search"
        name=""
        id=""
      />
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
  align-items: center;
  background-color: black;
  .searchBox {
    text-align: center;
    padding: 20px;
    input {
      padding: 10px;
      border: none;
    }
    button {
      padding: 10px;
      background-color: red;
      border: none;
      &:hover {
        background: rgb(172, 17, 17);
        cursor: pointer;
      }
    }
  }
}
</style>