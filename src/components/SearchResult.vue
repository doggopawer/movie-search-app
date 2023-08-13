<template>
  <div class="search__result">
    <div
      class="search__item"
      v-for="searchItem in searchList"
      :key="searchItem.imdbID"
      :id="searchItem.imdbID"
      @click="handleOpenModal"
    >
      <img v-if="searchItem.Poster !== 'N/A'" v-bind:src="searchItem.Poster" />
      <img v-else src="../assets/no_image.png" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "SearchResult",
  props: ["searchList"],
  methods: {
    handleOpenModal(event: Event) {
      this.$emit("handleOpenModal", event);
    },
    handleCloseModal(event: Event) {
      this.$emit("handleCloseModal", event);
    },
  },
});
</script>

<style>
.search__result {
  max-width: 1200px;
  margin: 0 auto;
  margin-top: 18px;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 20px;
}
.search__item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 20px;
}
</style>
