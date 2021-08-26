<template>
  <div>
    <v-card-text class="pa-0 ma-0">
      <div class="font-weight-bold ml-4">List Table</div>
    </v-card-text>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table :headers="headers" :items="results" :search="search">
      <template #[`item.timestamp`]="{ item }">
        {{ item.timestamp | formatDatetime }}
      </template>
      <template #[`item.id`]="{ item }">
        {{ item.id }}
      </template>
    </v-data-table>
  </div>
</template>

<script>
import axios from "axios";
const moment = require("moment");
export default {
  filters: {
    formatDatetime(value) {
      return moment(value).format("YYYY-MM-DD  HH:MM:ss");
    },
  },
  data: () => ({
    search: "",
    results: [],
    headers: [
      { text: "ID", value: "id" },
      { text: "Data", value: "data" },
      { text: "Data2", value: "data2" },
      { text: "Timestamp", value: "timestamp" },
    ],
  }),
  mounted() {
    axios.get("https://swdapi.ddns.net:8090/data/ttntest").then((response) => {
      const result = response.data;
      function getUniqueListBy(result, key) {
        return [...new Map(result.map((item) => [item[key], item])).values()];
      }
      const arr2 = getUniqueListBy(result, "id");
      this.results = arr2;
    });
  },
};
</script>

<style></style>
