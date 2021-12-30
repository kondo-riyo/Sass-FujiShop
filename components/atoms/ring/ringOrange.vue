<template>
  <div
    class="ring__orange"
    v-if="item.imgOrange"
    @click="selectRing(item.imgOrange)"
  >
    <span class="ring__orange__incircle"> </span>
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
