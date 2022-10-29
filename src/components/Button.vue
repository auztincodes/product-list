<template>
  <button
    :type="submitType"
    :class="classes"
    class="btn"
    :style="style"
    v-bind="$attrs"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: "button-component",
  props: {
    buttonType: {
      type: String,
      default: "primary",
      validator: function (value) {
        return ["primary", "secondary"].indexOf(value) !== -1;
      },
    },
    submitType: {
      type: String,
      default: "button",
    },
    width: {
      type: String,
      default: "",
    },
  },
  computed: {
    classes() {
      return {
        [`btn-${this.buttonType}`]: true,
      };
    },
    style() {
      return {
        width: this.width,
      };
    },
  },
};
</script>
<style lang="scss" scoped>
.btn {
  height: 38px;
  padding: 0 16px;
  border-radius: 4px;
  cursor: pointer;
  text-transform: capitalize;
  color: #fff;
  font-size: 0.75rem;

  &-primary {
    background: #377364;
    color: #fff;
  }
  &-secondary {
    background: #375673;
  }
}
</style>
