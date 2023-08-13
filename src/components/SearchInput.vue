<template>
  <div class="search__input">
    <input
      :value="searchText"
      @input="handleChangeSearchText"
      placeholder="Search Movie or Program"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "SearchInput",
  props: ["searchText"],
  methods: {
    async handleChangeSearchText(event: Event) {
      this.$emit("handleChangeSearchText", event);
    },
  },
});
</script>

<style>
.search__input {
  max-width: 1200px;
  margin: 0 auto;
}
.search__input input {
  box-sizing: border-box;
  width: 100%;
  height: 60px;
  padding: 5px 14px 0px 45px;
  background-color: #10161d;
  color: #fff;
  font-size: 24px;
  font-family: "Roboto Mono", monospace;
  border: 1px solid black;
  outline: none;
}
</style>
