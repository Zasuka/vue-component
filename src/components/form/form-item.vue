<template>
  <div class="main">
    <label v-if="label">{{ label }} </label>
    <div>
      <slot></slot>
      <div v-if="validateState === 'error'" class="i-form-item-message">
        {{ validateMessage }}
      </div>
    </div>
  </div>
</template>

<script>
import emitter from "../../mixins/emitter";
import AsyncValidator from "async-validator";
export default {
  name: "iFormItem",
  inject: ["form"],
  props: {
    label: {
      type: String,
      default: "",
    },
    prop: {
      type: String,
    },
  },
  data() {
    return {
      validateState: "", // 校验状态
      validateMessage: "", // 校验不通过时的提示信息
    };
  },
  computed: {
    fieldValue() {
      return this.form.model[this.prop];
    },
  },
  methods: {
    setRules() {
      emitter.on("on-form-change", this.onFieldChange);
      emitter.on("on-form-blur", this.onFieldBlur);
    },
    getRules() {
      let formRules = this.form.rules;

      formRules = formRules ? formRules[this.prop] : [];
      return [].concat(formRules || []);
    },
    getFilteredRule(trigger) {
      const rules = this.getRules();

      return rules.filter(
        (rule) => !rule.trigger || rule.trigger.indexOf(trigger) !== -1
      );
    },
    /**
     * 校验数据
     * @param trigger 校验类型
     * @param callback 回调函数
     */
    validate(trigger, callback = function () {}) {
      let rules = this.getFilteredRule(trigger);

      if (!rules || rules.length === 0) {
        return true;
      }

      // 设置状态为校验中
      this.validateState = "validating";

      // 以下为 async-validator 库的调用方法
      let descriptor = {};
      descriptor[this.prop] = rules;

      const validator = new AsyncValidator(descriptor);
      let model = {};

      model[this.prop] = this.fieldValue;

      validator.validate(model, { firstFields: true }, (errors) => {
        this.validateState = !errors ? "success" : "error";
        this.validateMessage = errors ? errors[0].message : "";
        // console.log(this.validateState);
        callback(this.validateMessage);
      });
    },
    onFieldBlur() {
      this.validate("blur");
    },
    onFieldChange() {
      this.validate("change");
    },
  },
  mounted() {
    if (this.prop) {
      emitter.emit("on-form-item-add", this);
      this.setRules();
    }
  },
  beforeDestroy() {
    emitter.emit("on-form-item-remove", this);
  },
};
</script>

<style  scoped>
.main {
  display: flex;
}
</style>

