<template>
  <input
    @input="onInput"
    class="input__search"
  />
</template>
<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  methods: {
    onInput(e:string): void {
        //@ts-ignore
        this.$emit('input', e.target.value);
    },
  },
});
</script>
<style lang="scss">
@import "../../../style/atoms/input.scss";
</style>
