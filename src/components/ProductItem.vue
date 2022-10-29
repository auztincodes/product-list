<template>
  <div class="content">
    <div class="content__product-details">
      <img :src="source" alt="" class="product-image" />
      <div><div class="status" :class="getIndicatorColor(status)"></div></div>
      <p class="product-name">{{ title }}</p>
    </div>
    <div class="content__product-date">
      <p class="date">
        <template v-if="date">{{ format(date, "dd/MM/yy") }}</template>
      </p>
      <slot name="dropdown"></slot>
    </div>
  </div>
</template>

<script>
import { format } from "date-fns";
export default {
  name: "product-item",
  data() {
    return {
      format,
    };
  },
  props: {
    source: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    status: {
      type: String,
      required: true,
    },
    date: {
      type: Number,
      required: false,
    },
  },
  methods: {
    getIndicatorColor(color) {
      switch (color) {
        case "green":
          return `status-${color}`;
        case "yellow":
          return `status-${color}`;
        default:
          return "status-red";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/main.scss";
.status {
  width: 9.33px;
  height: 9.33px;
  border-radius: 4.665px;
  margin-left: 11.33px;
  margin-right: 11.33px;
  &-green {
    background-color: #518103;
  }
  &-red {
    background-color: #c63434;
  }
  &-yellow {
    background-color: #e4d33a;
  }
}
.content,
.content__product-date {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.content__product-details {
  display: flex;
  align-items: center;
}
.content {
  border-bottom: 1px solid #e0e2e4;
  padding: 16px 0;
  @include respond-above("md") {
    width: 48%;
  }
}
.product-image {
  width: 24px;
  height: 24px;
  border-radius: 2px;
}
.date {
  margin-right: 11.34px;
}
</style>
