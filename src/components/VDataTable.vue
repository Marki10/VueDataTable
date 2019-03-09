<template>
  <div class="data-table">
    <div class="data-table__inner">
      <ul class="table-header">
        <li v-for="(item, key) in columns" :key="key">
          <div class="col" @click="sort(item.dataKey);">{{ item.name }}</div>
        </li>
      </ul>
      <ul class="table-content">
        <li class="row" v-for="(item, key) in sortedRows" :key="key">
          <div class="col">{{ item.code }}</div>
          <div class="col">{{ item.startDate }}</div>
          <div class="col">{{ item.sales }}</div>
        </li>
      </ul>
    </div>
    <div class="pagination">
      <div class="button arrow-left" @click="prevPage"><</div>
      <div class="numbers">
        <span
          class="button"
          @click="changeCurrentPage(item);"
          v-for="item in this.paginationNumbers()"
          >{{ item }}</span
        >
      </div>
      <div class="button arrow-right" @click="nextPage">></div>
      <div class="pages">{{ numberOfPages + "page" }}</div>
      <div class="result">{{ "321" }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VDataTable",
  props: ["columns", "rows", "perPage"],
  data: function() {
    return {
      currentSort: "code",
      currentSortDir: "asc",
      pageSize: this.perPage,
      currentPage: 1,
      numberOfPages: Math.ceil(this.perPage / this.columns.length)
    };
  },
  methods: {
    sort: function(s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
    },
    nextPage: function() {
      if (this.currentPage < this.numberOfPages) this.currentPage++;
    },
    prevPage: function() {
      if (this.currentPage > 1) this.currentPage--;
    },
    changeCurrentPage: function(newPage) {
      this.currentPage = newPage;
    },
    paginationNumbers: function() {
      let pages = [];
      for (let i = 0; i < this.numberOfPages; i++) {
        pages.push(i + 1);
      }
      return pages;
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
$background: #f6f7f8;
$even-color: #e6ebec;

.data-table {
  margin: 5px;

  &__inner {
    background: $background;
  }
}

ul {
  padding: 0;
  margin: 0;

  li {
    list-style-type: none;
  }
}

.table-header {
  display: flex;
}

.row {
  display: flex;

  &:nth-child(even) {
    background: $even-color;
  }
}
.col {
  padding: 10px;
}

.pagination {
  width: 100%;
  padding: 5px;
  display: flex;
}
.button {
  width: 25px;
  height: 25px;
  padding: 2px 0 0 8px;
  box-sizing: border-box;
  background: $even-color;
  border: 1px solid #171;
}

.arrow-left {
  border-radius: 5px 0 0 5px;
}
.arrow-right {
  border-radius: 0 5px 5px 0;
}
.numbers {
  display: flex;
}
.pages {
  padding: 5px;
  color: #555;
}
.result {
  padding: 5px;
  color: #555;
}
</style>
