<template>
  <div>
    <i-form :model="formData" :rules="rule">
      <i-form-item label="用户名" prop="name">
        <i-input v-model="formData.name"></i-input>
      </i-form-item>
      <i-form-item label="邮箱" prop="mail">
        <i-input v-model="formData.mail"></i-input>
      </i-form-item>
    </i-form>
  </div>
</template>

<script>
import iForm from "../components/form/form.vue";
import iFormItem from "../components/form/form-item.vue";
import iInput from "../components/form/input.vue";
export default {
  components: {
    iForm,
    iFormItem,
    iInput,
  },
  data() {
    return {
      formData: {
        name: "",
        mail: "",
      },
      rule: {
        name: [{ required: true, message: "用户名不能为空", trigger: "blur" }],
        mail: [
          { required: true, message: "邮箱不能为空", trigger: "blur" },
          { type: "email", message: "邮箱格式不正确", trigger: "blur" },
        ],
      },
    };
  },

  created() {},

  mounted() {
    // console.log(this.$listeners);
  },
};
</script>

<style>
</style>