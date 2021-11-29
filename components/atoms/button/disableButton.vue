<template>
    <!-- <button
      class="
        bg-gray-400
        text-gray-500
        font-bold
        py-2
        px-6
        rounded-lg
        "
        ><slot />
    </button> -->
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
@import "../../../style/disable-button.scss";
// $gray-400: #e5e7eb;
// $gray-500: #6b7280;
// .disable-button {
//   background-color: $gray-400;
//   color: $gray-500;
//   font-weight: bold;
//   padding: 0.5rem 1.5rem;
//   border-radius: 0.5rem;
//   // padding-top: 0.5rem;
//   // padding-bottom: 0.5rem;
//   // padding-left: 1.5rem;
//   // padding-right: 1.5rem;
// }
</style>