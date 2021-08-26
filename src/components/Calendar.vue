<template>
  <div>
    <v-card-text class="pa-0 ma-0">
      <div class="font-weight-bold ml-4">List Calendar</div>
    </v-card-text>
    <v-row>
      <v-card-text class="pa-0 ma-0">
        <div class="text-h5 font-weight-bold ml-7 mt-4">{{ dateHeader }}</div>
      </v-card-text>
      <v-col>
        <v-sheet height="500">
          <v-calendar :value="today" color="primary">
            <template v-slot:day="{ past, date }">
              <template v-if="past && tracked[date]">
                <v-sheet
                  v-for="(percent, i) in tracked[date]"
                  :key="i"
                  :title="category[i]"
                  :color="colors[i]"
                  >{{ percent }}</v-sheet
                >
              </template>
            </template>
          </v-calendar>
        </v-sheet>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from "axios";
const moment = require("moment");
export default {
  data: () => ({
    addDay: "",
    dateHeader: "",
    today: "",
    tracked: {},
    colors: ["cyan", "green"],
    category: ["data", "data2"],
  }),
  mounted() {
    axios.get("https://swdapi.ddns.net:8090/data/ttntest").then((response) => {
      const result = response.data;
      function getUniqueListBy(result, key) {
        return [...new Map(result.map((item) => [item[key], item])).values()];
      }
      const arr2 = getUniqueListBy(result, "id");
      this.results = arr2;
      function groupBy(list, keyGetter) {
        const map = new Map();
        list.forEach((item) => {
          const key = keyGetter(item);
          const collection = map.get(key);
          if (!collection) {
            map.set(key, [item]);
          } else {
            collection.push(item);
          }
        });
        return map;
      }
      const grouped = groupBy(this.results, (data) =>
        data.timestamp.substring(0, 10)
      );
      grouped.forEach((value, key) => {
        const data = [];
        const data2 = [];
        value.forEach((element) => {
          data.push(element.data);
          if (element.data2 !== null) {
            data2.push(element.data2);
          }
        });
        const length = "data: " + data.length;
        const length2 = "data2: " + data2.length;
        this.tracked[key] = [length, length2];
        this.addDay = key;
      });
      const startdate = this.addDay;
      const new_date = moment(startdate, "YYYY-MM-DD").add(1, "days");
      const day = new_date.format("DD");
      const month = new_date.format("MM");
      const year = new_date.format("YYYY");
      this.today = year + "-" + month + "-" + day;
      this.dateHeader = moment(this.today).format("MMMM YYYY");
    });
  },
};
</script>

<style></style>
