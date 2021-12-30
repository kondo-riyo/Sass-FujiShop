<template>
  <div
    class="ring__red"
    v-if="item.imgRed"
    @click="selectRing(item.imgRed)"
  >
    <span class="ring__red__incircle"></span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props:['item'],
  methods: {
    selectRing(img:string):void {
      this.$emit("selectRing",img);
    }
  }
})
</script>
<style lang="scss">
@import "../../../style/atoms/ring.scss";

</style>
