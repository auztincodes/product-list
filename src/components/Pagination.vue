<template>
  <div class="pagination-wrapper">
    <div>
      <span class="page-nav" @click="$emit('prevPage')"
        ><img
          src="../assets/icons/Chevron-down.svg"
          alt="chevron-left"
          svg-inline
          class="chevron chevron-left"
          width="9.32"
          height="4.78"
      /></span>
      <span class="current-page"
        >Page <span>{{ currentPage }} </span>of
        <span>{{ totalPage }}</span></span
      >
      <span class="page-nav" @click="$emit('nextPage')"
        ><img
          src="../assets/icons/Chevron-down.svg"
          alt="chevron-right"
          svg-inline
          class="chevron chevron-right"
          width="9.32"
          height="4.78"
      /></span>
    </div>
    <p @click="$emit('sortProduct')">
      Sort by <span class="sort-order">{{ sortDir }}</span>
    </p>
    <div class="page-wrapper">
      <span class="per-page">{{ perPage }}</span>
      <toggle-dropdown>
        <template #dropdown-wrapper>
          <img
            src="../assets/icons/Chevron-down.svg"
            svg-inline
            class="chevron"
          />
        </template>
        <template #dropdown-items>
          <li class="dropdown__item" @click="$emit('changePerPage', 10)">10</li>
          <li class="dropdown__item" @click="$emit('changePerPage', 15)">15</li>
          <li class="dropdown__item" @click="$emit('changePerPage', 20)">20</li>
        </template>
      </toggle-dropdown>
    </div>
  </div>
</template>
<script>
import ToggleDropdown from "./ToggleDropdown.vue";

export default {
  name: "pagination-component",
  components: { ToggleDropdown },
  props: {
    totalPage: {
      type: Number,
      required: true,
    },
    total: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
    currentPage: {
      type: Number,
      required: true,
    },
    sortDir: {
      type: String,
      required: true,
    },
  },
};
</script>
<style lang="scss" scoped>
.pagination-wrapper {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  text-align: center;
}
.page-nav {
  padding: 0 5px;
  cursor: pointer;
}
.current-page {
  margin: 0 5px;
  span {
    font-weight: 700;
  }
}
.sort-order,
.per-page {
  font-weight: 700;
  text-transform: capitalize;
}
.page-wrapper {
  display: flex;
  align-items: center;
}
.per-page {
  margin-right: 10px;
}
</style>
