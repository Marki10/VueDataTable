<template>
  <ul class="table-content" v-bind:style="{ minHeight: 55 * perPage + 'px' }">
    <li class="table-content__row" v-for="(item, key) in rows" :key="key">
      <div
        class="table-content__col"
        v-bind:style="{ 'text-align': columns[0].align }"
      >
        {{ item.code }}
      </div>
      <div class="table-content__col">{{ "Boundless magazine" }}</div>
      <div
        class="table-content__col"
        v-bind:style="{ 'text-align': columns[2].align }"
      >
        {{ columns[2].formatValue(item.startDate) }}
        <!-- { - { item.startDate | dateFormat } - } -->
      </div>
      <div class="table-content__col">
        {{ new Date(2019, 2, 15) | dateFormat }}
      </div>
      <div
        class="table-content__col"
        v-bind:style="{ 'text-align': columns[4].align }"
      >
        {{ columns[4].formatValue(item.sales) }}
        <!-- { - { item.sales | currencyFormat } - } -->
      </div>
      <div class="table-content__col">{{ "xxx" }}</div>
      <div class="table-content__col">{{ "xxx" }}</div>
    </li>
  </ul>
</template>

<script>
import { format } from "date-fns";

export default {
  name: "TableContent",
  props: ["rows", "perPage", "columns"],
  filters: {
    dateFormat: function(value) {
      return format(value, "DD MMM YYYY");
    },
    currencyFormat: function(value) {
      return "£" + value.toLocaleString("en-GB");
    }
  }
};
</script>

<style lang="scss">
@import "../assets/styles/variables.scss";

.table-content {
  &__row {
    &:nth-child(even) {
      background: $even-color;
    }
  }
}
</style>
