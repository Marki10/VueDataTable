<template>
  <div class="data-table">
    <div class="data-table__inner">
      <TableHeader :columns="columnsExtraFields()" />
      <TableContent
        :rows="sortedRows"
        :per-page="perPage"
        :columns="columnsExtraFields()"
      />
    </div>
    <Pagination :numberOfPages="numberOfPages" :rowsLength="this.rows.length" />
  </div>
</template>

<script>
import TableHeader from "./TableHeader";
import TableContent from "./TableContent";
import Pagination from "./Pagination";

export default {
  name: "VDataTable",
  props: ["columns", "rows", "perPage"],
  components: {
    TableHeader,
    TableContent,
    Pagination
  },
  data: function() {
    return {
      currentSort: "code",
      currentSortDir: "asc",
      pageSize: this.perPage,
      numberOfPages: this.getNumberOfPages(),
      currentPage: 1
    };
  },
  methods: {
    getNumberOfPages: function() {
      return Math.ceil(this.rows.length / this.perPage);
    },
    setCurrentPage: function(current) {
      this.currentPage = current;
    },
    sort: function(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
    },
    columnsExtraFields: function() {
      let newColumns = [];
      for (let i = 0; i < this.columns.length; i++) {
        newColumns.push(this.columns[i]);
        if (this.columns[i].dataKey === "code") {
          newColumns.push({
            dataKey: "media",
            name: "Media",
            align: "left"
          });
        }
        if (this.columns[i].dataKey === "startDate") {
          newColumns.push({
            dataKey: "lastOrder",
            name: "Last Order",
            align: "left"
          });
        }
      }
      newColumns.push({ dataKey: "ltv", name: "LTV", align: "left" });
      newColumns.push({ dataKey: "cac", name: "CAC", align: "left" });
      return newColumns;
    }
  },
  computed: {
    sortedRows: function() {
      return this.rows
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
    }
  }
};
</script>

<style lang="scss">
@import "../assets/styles/variables.scss";
@import "../assets/styles/reset.scss";
@import "./VDataTable.scss";
</style>
