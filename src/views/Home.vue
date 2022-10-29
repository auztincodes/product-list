<template>
  <div class="wrapper">
    <div class="container">
      <h2 class="header">Products</h2>
      <main class="main">
        <div class="forms">
          <div class="input-wrapper">
            <Input
              placeholder="Find a Product"
              v-model="searchValue"
              @keyup.enter="searchProduct"
            />
            <Button buttonType="secondary" @click="searchProduct"
              >Search</Button
            >
          </div>
          <div class="input-wrapper">
            <Input placeholder="Product Name" />
            <Button width="140px">Add product</Button>
          </div>
        </div>

        <div class="products">
          <product-item
            v-for="(item, index) in sortedProducts"
            :key="index"
            :title="item.name"
            :source="item.image"
            :status="item.status"
            :date="item.date"
          >
            <template #dropdown>
              <toggle-dropdown>
                <template #dropdown-wrapper>
                  <img
                    src="../assets/icons/Chevron-down.svg"
                    svg-inline
                    class="chevron"
                  />
                </template>
                <template #dropdown-items>
                  <li
                    class="dropdown__item"
                    :class="{ success: item.status === 'green' }"
                    @click="changeStatus(item, index, 'green')"
                  >
                    Success
                  </li>
                  <li
                    class="dropdown__item"
                    :class="{ warning: item.status === 'yellow' }"
                    @click="changeStatus(item, index, 'yellow')"
                  >
                    Warning
                  </li>
                  <li
                    class="dropdown__item"
                    :class="{ fail: item.status === 'red' }"
                    @click="changeStatus(item, index, 'red')"
                  >
                    Fail
                  </li>
                </template>
              </toggle-dropdown>
            </template>
          </product-item>
        </div>
      </main>
      <footer>
        <pagination
          :total-page="totalPage"
          :total="products.length"
          :per-page="pageSize"
          :current-page="currentPage"
          :sort-dir="currentSortDir"
          @sortProduct="sort('name')"
          @prevPage="prevPage"
          @nextPage="nextPage"
          @changePerPage="changePerPage"
        />
      </footer>
    </div>
  </div>
</template>
<script>
import { products } from "../data";
import Button from "@/components/Button.vue";
import Input from "@/components/Input.vue";
import ProductItem from "../components/ProductItem.vue";
import Pagination from "../components/Pagination.vue";
import ToggleDropdown from "../components/ToggleDropdown.vue";
export default {
  name: "HomeView",
  components: {
    ProductItem,
    Button,
    Input,
    Pagination,
    ToggleDropdown,
  },
  data() {
    return {
      products,
      searchValue: "",
      currentSort: "name",
      currentSortDir: "asc",
      pageSize: 10,
      currentPage: 1,
    };
  },
  watch: {
    searchValue(value) {
      if (value === "") {
        this.products = products;
      }
    },
  },
  methods: {
    sort(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
    },
    nextPage() {
      if (this.currentPage * this.pageSize < this.products.length)
        this.currentPage++;
    },
    prevPage() {
      if (this.currentPage > 1) this.currentPage--;
    },
    changePerPage(page) {
      this.pageSize = page;
    },
    changeStatus(product, index, status) {
      const position = this.products.indexOf(product);
      if (position !== -1) {
        this.products[index] = { ...product, status: status };
      }
    },
    searchProduct() {
      this.products = this.products.filter((item) => {
        return item.name.toUpperCase().includes(this.searchValue.toUpperCase());
      });
    },
  },
  computed: {
    totalPage() {
      return this.products.length < this.pageSize
        ? 1
        : Math.ceil(this.products.length / this.pageSize);
    },
    sortedProducts() {
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      return this.products
        .sort((a, b) => {
          let modifier = 1;
          if (this.currentSortDir === "desc") modifier = -1;
          if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
          return 0;
        })
        .filter((row, index) => {
          let start = (this.currentPage - 1) * this.pageSize;
          let end = this.currentPage * this.pageSize;
          if (index >= start && index < end) return true;
        });
    },
  },
};
</script>
<style lang="scss" scoped>
@import "../assets/scss/main.scss";
.wrapper {
  padding: 1rem 0 3rem;
}
.header {
  font-size: 1.5rem;
  font-family: "Bitter", serif;
  line-height: 2rem;
}
.main {
  background: #ffffff;
  border: 1px solid #e0e2e4;
  border-radius: 8px;
  padding: 1rem;
  margin: 1rem 0 0.5rem;
}
.input-wrapper {
  display: flex;
  align-items: center;
  gap: 1ch;
  margin-bottom: 0.5rem;
  @include respond-above("md") {
    width: 48%;
  }
}
.products,
.forms {
  @include respond-above("md") {
    display: flex;
    flex-wrap: wrap;
    gap: 2ch;
  }
}
.dropdown__item {
  margin: 0;
  position: relative;
  padding: 12px 16px;
  cursor: pointer;
}
.fail {
  color: #c63434;
}
.warning {
  color: #e4d33a;
}
.success {
  color: #518103;
}
</style>
