<template>
  <div class="u-relative">
    <span class="dropdown__wrapper" @click="showDropdown = !showDropdown">
      <slot name="dropdown-wrapper"></slot>
    </span>
    <template v-if="showDropdown">
      <div class="dropdown__list-wrapper" id="dropdown__list-wrapper">
        <ul id="dropdown-list" tabindex="-1" class="dropdown__list">
          <slot name="dropdown-items"></slot>
        </ul>
      </div>
    </template>
  </div>
</template>
<script>
export default {
  name: "ToggleDropdown",
  data() {
    return {
      showDropdown: false,
    };
  },
  methods: {
    onClose() {
      this.showDropdown = false;
    },
  },
  watch: {
    showDropdown() {
      this.$nextTick(() => {
        const dropdownList = Array.from(
          document.getElementsByClassName("dropdown__item")
        );
        for (const element of dropdownList) {
          element.addEventListener("click", () => {
            this.showDropdown = false;
          });
        }

        const dropdown = document.getElementById("dropdown-list");
        if (dropdown) {
          dropdown.addEventListener("blur", () => {
            setTimeout(() => {
              this.showDropdown = false;
            }, 200);
          });
          dropdown.focus();
        }
      });
    },
  },
};
</script>
<style lang="scss">
.dropdown {
  &__wrapper {
    padding: 1rem;
    display: flex;
    align-items: center;
    cursor: pointer;
  }
  &__btn-wrapper {
    display: flex;
    background-color: #f8f9fb;
    border-radius: 3px;
    position: relative;
  }
  &__text {
    color: #85879b;
  }
  &__item {
    margin: 0;
    position: relative;
    padding: 12px 16px;
    cursor: pointer;
    &:hover {
      font-weight: 700;
    }
  }
  &__list {
    padding: 0;
    margin: 5px 0;
    border-radius: 3px;
    box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.1);
    background-color: #ffffff;
    &-wrapper {
      position: absolute;
      top: 135%;
      right: 0;
      z-index: 100;
      background: #fff;
      min-width: 100px;
    }
    &:focus {
      outline: none;
    }
  }
}
</style>
