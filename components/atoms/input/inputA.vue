<template>
  <input
    :value="value"
    @input="onInput"
    class="input__base"
      :type="type"
      :name="name"
      :placeholder="placeholder"
 />
</template>
<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  name: 'inputA',
  props: {
    value: { type: String, required: false },
    type: { type: String, required: true },
    name: { type: String, required: true },
    placeholder: { type: String, required: false }
  },
  methods: {
    onInput(e: Event):void{
      //@ts-ignore
      this.$emit("input",e.target.value)
    }
  }
})
</script>
<style lang="scss">
@import "../../../style/atoms/input.scss";
</style>