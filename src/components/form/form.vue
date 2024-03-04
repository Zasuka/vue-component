
<template>
  <form>
    <slot></slot>
  </form>
</template>

<script>
import emitter from "@/mixins/emitter";

export default {
  name: "iForm",
  provide() {
    return {
      form: this,
    };
  },
  props: {
    model: {
      type: Object,
    },
    rules: {
      type: Object,
    },
  },
  data() {
    return {
      fields: [],
    };
  },
  created() {
    emitter.on("on-form-item-add", (field) => {
      if (field) this.fields.push(field);
    });
    emitter.on("on-form-item-remove", (field) => {
      if (field.prop) this.fields.splice(this.fields.indexOf(field), 1);
    });
  },

  mounted() {
    console.log(this.fields);
  },
};
</script>

<style>
</style>