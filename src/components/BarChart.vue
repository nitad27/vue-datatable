<script>
import { Bar } from "vue-chartjs";
import axios from "axios";

export default {
  extends: Bar,
  data() {
    return {
      labels: [],
      value: [],
      value2: [],
      results: [],
    };
  },
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
        this.labels.push(key);
        const data = [];
        const data2 = [];
        value.forEach((element) => {
          data.push(element.data);
          if (element.data2 !== null) {
            data2.push(element.data2);
          }
        });
        // console.log("sum", data.reduce((a, b) => a + b, 0));
        // console.log("length", data.length);
        // console.log("/", data.reduce((a, b) => a + b, 0)/data.length);
        // console.log("sum", data2.reduce((a, b) => a + b, 0));
        // console.log("length", data2.length);
        // console.log("/", data2.reduce((a, b) => a + b, 0)/data2.length);
        this.value.push(data.reduce((a, b) => a + b, 0) / data.length);
        this.value2.push(data2.reduce((a, b) => a + b, 0) / data2.length);
      });
      this.renderChart(
        {
          labels: this.labels,
          datasets: [
            {
              label: "Data",
              backgroundColor: "#f87979",
              data: this.value,
            },
            {
              label: "Data2",
              backgroundColor: "#FC2525",
              data: this.value2,
            },
          ],
        },
        { responsive: true, maintainAspectRatio: false }
      );
    });
  },
};
</script>
