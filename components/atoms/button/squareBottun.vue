<template>
    <button
      class="square-button"
        @click="click"
    ><slot />
    </button>    
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  methods: {
    click():void {
      this.$emit("click");
    }
  }
})
</script>
<style lang="scss">
  @import "../../../style/atoms/button.scss";

</style>