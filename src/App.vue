<template>
  <div id="app">
    <v-data-table :columns="columns" :rows="extraContent" :per-page="9" />
  </div>
</template>

<script>
import VDataTable from "./components/VDataTable";
import { format } from "date-fns";

export default {
  name: "App",
  components: {
    VDataTable
  },
  methods: {
    generateExtraContent: function() {
      let array = [];
      for (let i = 0; i < 150; i++) {
        let item = {
          code: "Camar" + i,
          startDate: new Date(2019, 2, Math.floor(Math.random() * 20)),
          sales: Math.floor(Math.random() * 9991)
        };

        array.push(item);
      }
      return array;
    }
  },
  data() {
    return {
      extraContent: this.generateExtraContent(),
      columns: [
        {
          dataKey: "code",
          name: "Code",
          align: "left"
        },
        {
          dataKey: "startDate",
          name: "Start Date",
          align: "left",
          formatValue(value) {
            return format(value, "DDDD MMMM YYYY");
          }
        },
        {
          dataKey: "sales",
          name: "Sales",
          align: "right",
          formatValue(value) {
            return "£" + value.toLocaleString("en-GB");
          }
        }
      ],
      campaigns: [
        {
          code: "CAM1",
          startDate: new Date(2019, 2, 15),
          sales: 3209
        },
        {
          code: "CAM2",
          startDate: new Date(2019, 1, 15),
          sales: 42469
        },
        {
          code: "CAM3",
          startDate: new Date(2019, 2, 10),
          sales: 469
        },
        {
          code: "CAM4",
          startDate: new Date(2019, 2, 9),
          sales: 11203
        },
        {
          code: "CAM5",
          startDate: new Date(2019, 0, 1),
          sales: 123
        },
        {
          code: "CAM6",
          startDate: new Date(2019, 0, 15),
          sales: 0
        },
        {
          code: "CAM7",
          startDate: new Date(2019, 0, 10),
          sales: 8096
        }
      ]
    };
  }
};
</script>

<style lang="scss">
@import "./assets/styles/variables.scss";

body {
  background: $background;
  font-size: 15px;
  font-family: sans-serif;
}
</style>
