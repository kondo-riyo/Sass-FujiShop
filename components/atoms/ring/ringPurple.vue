<template>
  <div
    class="ring__purple"
    v-if="item.imgPurple"
    @click="selectRing(item.imgPurple)"
  >
    <span class="ring__purple__incircle"> </span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  props: ['item'],
  methods: {
    selectRing(img: string):void {
      this.$emit('selectRing', img);
    },
  },
});
</script>
<style lang="scss">
@import "../../../style/atoms/ring.scss";

</style>