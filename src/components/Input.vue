<template>
  <input
    :id="name"
    ref="input"
    v-model="innerValue"
    class="form-input"
    :type="type"
    :placeholder="placeholder"
  />
</template>
<style lang="scss" scoped>
.form-input {
  width: 100%;
  height: 38px;
  border: 1px solid #e0e2e4;
  border-radius: 4px;
  padding: 8px 12px;
  color: #252526;
  &::placeholder {
    color: #939597;
  }
  &:focus,
  &:focus-within {
    outline: none;
  }
}
</style>
<script>
export default {
  name: "input-component",
  props: {
    name: {
      type: String,
      default: "",
    },
    placeholder: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "text",
    },
    value: {
      type: String,
      default: "",
    },
  },
  data: () => ({
    innerValue: "",
  }),
  watch: {
    innerValue(value) {
      this.$emit("input", value);
    },
    value(val) {
      if (val !== this.innerValue) {
        this.innerValue = val;
      }
    },
  },
};
</script>
