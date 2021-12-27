<template>
  <fieldset>
    <select 
     :name="name" 
     @change="updateValue"
     class="input__base"
    >
        <option v-for="(option, index) in options" :value="option.value" :key="index">
          {{ option.label }}
        </option>
    </select>
  </fieldset>
</template>

<script >
export default {
  name: "deliverySelect",
  props: {
    value: { type: String, require: true },
    options: { type: Array, require: true },
    name: { type: String, require: true }
  },
  methods: {
    updateValue (e){
      this.$emit("input", e.target.value);
    },
    mountedEmit() {
      this.$emit("input", this.options[0].value);
    }
  },
  mounted() {
    // this.$emit("input", this.options[0].value);
    this.mountedEmit()
  }
};
</script>
<style lang="scss">
@import "../../../style/atoms/input.scss";

</style>