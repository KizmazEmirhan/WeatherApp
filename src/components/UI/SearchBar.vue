<template>
  <div class="flex items-center justify-end">
    <input
      type="text"
      class="w-full rounded-lg h-10 p-3 text-sm"
      id="searchPlace"
      placeholder="Type a place for check weather condition"
      v-model="searchedPlace"
      @keydown.enter="checkSearchedPlace"
    />
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="26"
      height="26"
      fill="currentColor"
      class="bi bi-search"
      viewBox="0 0 16 16"
      @click="checkSearchedPlace"
    >
      <path
        d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001q.044.06.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1 1 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0"
      />
    </svg>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchedPlace: "",
    };
  },
  methods: {
    checkSearchedPlace() {
      this.$emit("placeUpdated", this.searchedPlace);
     
    },
  },
};
</script>

<style scoped>
svg {
  position: absolute;
  margin-right: 6px;
  cursor: pointer;
}
</style>
