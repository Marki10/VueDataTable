<template>
  <div class="pagination">
    <div class="button arrow-left" @click="prevPage">&laquo;</div>
    <div class="numbers">
      <span
        class="button"
        v-bind:class="{ 'button--active': item === currentPage }"
        @click="changeCurrentPage(item);"
        v-for="item in this.paginationNumbers()"
        :key="item"
      >
        {{ item }}</span
      >
    </div>
    <div class="button arrow-right" @click="nextPage">&raquo;</div>
    <div class="content-info">
      {{ numberOfPages + " page" }}{{ numberOfPages > 1 ? "s" : "" }} ({{
        rowsLength + " result"
      }}{{ rowsLength > 1 ? "s" : "" }})
    </div>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ["numberOfPages", "rowsLength"],
  methods: {
    nextPage: function() {
      if (this.currentPage < this.numberOfPages) {
        this.currentPage++;
        if (this.currentPage > 9) {
          this.pagerPosition++;
        }
      }
    },
    prevPage: function() {
      if (this.currentPage > 1) {
        this.currentPage--;
        if (this.currentPage - this.pagerPosition < 1) {
          this.pagerPosition--;
        }
      }
    },
    changeCurrentPage: function(newPage) {
      this.currentPage = newPage;
    },
    paginationNumbers: function() {
      let pages = [];

      for (let i = 0 + this.pagerPosition; i < this.numberOfPages; i++) {
        pages.push(i + 1);
      }
      return pages.splice(0, 9);
    }
  },
  data: function() {
    return {
      currentPage: 1,
      pagerPosition: 0
    };
  },
  watch: {
    currentPage: function(current) {
      this.$parent.setCurrentPage(current);
    }
  }
};
</script>

<style lang="scss">
@import "../assets/styles/variables.scss";

.pagination {
  display: flex;
  margin: 40px 0 10px;
}

.numbers {
  display: flex;
}

.button {
  width: 25px;
  height: 25px;
  padding: 4px 0;
  text-align: center;
  box-sizing: border-box;
  background: #fff;
  font-size: 0.8rem;
  border: 1.5px solid $even-color;
  border-right: 0;
  cursor: pointer;

  &--active,
  &:hover {
    background: $even-color;
  }
}
.arrow-left {
  border-radius: 5px 0 0 5px;
}
.arrow-right {
  border-radius: 0 5px 5px 0;
  border-right: 1.5px solid $even-color;
}

.content-info {
  padding: 5px;
  margin-right: 20px;
  color: #6f6f6f;
}
</style>
