<template>
  <div class="table-header">
    <ul class="table-header__row">
      <li
        class="table-header__col"
        v-for="(item, key) in columns"
        :key="key"
        @click="sort(item.dataKey);"
      >
        <span>{{ item.name }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "TableHeader",
  props: ["columns"],
  methods: {
    sort(dataKey) {
      this.$parent.sort(dataKey);
    }
  }
};
</script>

<style lang="scss">
@import "../assets/styles/variables.scss";

.table-header {
  &__col {
    span {
      display: inline-block;
      position: relative;
      cursor: pointer;

      &::after {
        content: "";
        width: 15px;
        height: 15px;
        background: url($arrows) no-repeat;
        background-size: 10px 10px;
        display: inline-block;
        position: absolute;
        top: 5px;
        right: -30px;
      }
    }
  }
}
</style>
