<template>
  <div class="search">
    <SearchInput
      @handleChangeSearchText="handleChangeSearchText"
      :searchText="searchText"
    />
    <SearchResult
      :searchList="searchList"
      :isLoading="isLoading"
      @handleOpenModal="handleOpenModal"
    />
  </div>
  <div class="detail">
    <DetailModal
      :searchItem="searchItem"
      :modal="modal"
      :isLoading="isLoading"
      @handleCloseModal="handleCloseModal"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SearchInput from "./components/SearchInput.vue";
import SearchResult from "./components/SearchResult.vue";
import DetailModal from "./components/DetailModal.vue";
import axios from "axios";
import ResponseValue from "./types";
import ModalEventData from "./types";
import { API_KEY, API_URL } from "./constants";
export default defineComponent({
  name: "App",
  components: {
    SearchInput,
    SearchResult,
    DetailModal,
  },
  data() {
    return {
      searchText: "",
      searchList: [] as ResponseValue[],
      searchItem: {},
      currentPage: 1,
      totalPage: 0,
      isLoading: false,
      modal: false,
      timer: null as ReturnType<typeof setTimeout> | null,
    };
  },
  methods: {
    async handleChangeSearchText(event: InputEvent) {
      this.handleDebounce(async () => {
        const inputElement = event.target as HTMLInputElement;
        this.searchText = inputElement.value;
        const response = await axios.get(
          `${API_URL}?s=${this.searchText}&apikey=${API_KEY}`
        );
        const searchList = response.data.Search;
        this.searchList = searchList;
        this.totalPage = Math.floor(response.data.totalResults / 10) + 1;
        this.currentPage = 1;
      });
    },
    handleDebounce(action: () => Promise<void>) {
      const timeSetting = 1000;
      if (this.timer !== null) {
        clearTimeout(this.timer);
      }
      this.timer = setTimeout(action, timeSetting);
    },
    handleInfiniteScroll() {
      const isSearchListNotEmpty = this.searchList.length !== 0;
      if (isSearchListNotEmpty) {
        const scrollPosition = window.scrollY;

        const viewportHeight =
          window.innerHeight || document.documentElement.clientHeight;

        const documentHeight = document.documentElement.scrollHeight;

        if (scrollPosition + viewportHeight >= documentHeight) {
          if (this.currentPage < this.totalPage) {
            this.currentPage++;
            this.loadMoreData();
          }
        }
      }
    },
    async loadMoreData() {
      const response = await axios.get(
        `${API_URL}?s=${this.searchText}&page=${this.currentPage}&apikey=${API_KEY}`
      );

      this.searchList = this.searchList.concat(response.data.Search);
    },
    async handleOpenModal(event: MouseEvent) {
      event.stopPropagation();
      this.isLoading = true;
      this.modal = true;
      const currentTarget = event.currentTarget as HTMLInputElement;
      const response = await axios.get(
        `${API_URL}?i=${currentTarget.id}&apikey=${API_KEY}`
      );
      this.searchItem = response.data;
      this.isLoading = false;
    },
    async handleCloseModal(data: ModalEventData) {
      const { modalEl, target } = data;
      if (this.modal && !modalEl.contains(target as Node)) {
        this.modal = false;
        this.searchItem = {};
      }
    },
  },
  mounted() {
    window.addEventListener("scroll", this.handleInfiniteScroll);
  },
  unmounted() {
    window.addEventListener("scroll", this.handleInfiniteScroll);
  },
});
</script>

<style>
.search {
  width: 100%;
  margin-top: 20px;
}
.detail {
  max-width: 1200px;
  margin: 0 auto;
}
</style>
