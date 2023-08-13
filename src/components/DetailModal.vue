<template>
  <div class="detail__backdrop" v-show="modal"></div>
  <div class="detail__modal" v-show="modal" ref="modal">
    <template v-if="isLoading">
      <div class="detail__loading">
        <p>Wait a second...</p>
      </div>
    </template>
    <template v-else>
      <div class="detail__img">
        <img
          v-if="searchItem.Poster !== 'N/A'"
          v-bind:src="searchItem.Poster"
        />
        <img v-else src="../assets/no_image.png" />
      </div>
      <div class="detail__info">
        <div class="detail__essential">
          <div class="detail__title">{{ searchItem.Title }}</div>
          <div class="detail__year">({{ searchItem.Year }})</div>
        </div>
        <div class="detail__description">
          <div class="detail__plot">{{ searchItem.Plot }}</div>
        </div>
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "DetailModal",
  props: ["searchItem", "modal", "isLoading"],
  methods: {
    handleCloseModal(event: Event) {
      const modalEl = this.$refs.modal as HTMLElement;
      const target = event.target;
      this.$emit("handleCloseModal", { modalEl, target });
    },
  },
  mounted() {
    window.addEventListener("click", this.handleCloseModal);
  },
  unmounted() {
    window.removeEventListener("click", this.handleCloseModal);
  },
});
</script>

<style>
.detail__modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 90%;
  height: 80%;
  background-color: #060d17;
  color: #fff;
  border-radius: 20px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.2);
  display: flex;
}
.detail__backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* 검은색 배경, 투명도 조절 가능 */
}
.detail__img {
  width: 40%;
  height: 100%;
}
.detail__img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
}
.detail__info {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  width: 60%;
  padding: 30px;
}
.detail__essential {
  font-size: 3.5vw;
  font-family: "Roboto Mono", monospace;
}
.detail__description {
  margin-top: 20px;
  font-size: 2vw;
}
.detail__loading {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 5vw;
}
</style>
