<template>
        <img
        src="../../../assets/img/search.webp"
        class="search-logo__img"
        @click="search"
      />
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  methods: {
    search():void {
      this.$emit("search");
    }
  }
})
</script>
<style lang="scss">
.search-logo__img {
  display: block;
  width: 1.5rem;
  margin-right: auto;
  margin-top: 2.5rem;
}
</style>