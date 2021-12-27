<template>
    <button
      class="disable-button"
        ><slot />
    </button>
</template>
<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  name: 'disableButton',
})
</script>
<style lang="scss">
@import "../../../style/atoms/button.scss";
</style>