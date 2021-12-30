<template>
  <div
    class="ring__cream"
    v-if="item.imgCream"
    @click="selectRing(item.imgCream)"
  >
    <span class="ring__cream__incircle"> </span>
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