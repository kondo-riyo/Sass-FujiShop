<template>
  <div
    class="ring__brown"
    v-if="item.imgBrown"
    @click="selectRing(item.imgBrown)"
  >
    <span class="ring__brown__incircle"> </span>
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