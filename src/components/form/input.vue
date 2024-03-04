<template>
  <div>
    <input
      type="text"
      :value="currentValue"
      @input="handleInput"
      @blur="handleBlur"
    />
  </div>
</template>

<script>
import emitter from "@/mixins/emitter";

export default {
  name: "iInput",
  props: {
    value: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      currentValue: this.value,
    };
  },
  watch: {
    value(val) {
      this.currentValue = val;
    },
  },
  methods: {
    handleInput(event) {
      const value = event.target.value;
      this.currentValue = value;
      this.$emit("input", value);
      emitter.emit("on-form-change", value);
    },
    handleBlur() {
      emitter.emit("on-form-blur", this.currentValue);
    },
  },
};
</script>

<style>
</style>