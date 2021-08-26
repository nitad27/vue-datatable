<script>
import { Line } from "vue-chartjs";
import axios from "axios";

export default {
  extends: Line,
  data() {
    return {
      gradient: null,
      gradient2: null,
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
        // console.log(key);
        this.labels.push(key);
        const data = [];
        const data2 = [];
        value.forEach((element) => {
          data.push(element.data);
          if (element.data2 !== null) {
            data2.push(element.data2);
          }
        });
        // console.log("sum1", data.reduce((a, b) => a + b, 0));
        // console.log("length1", data.length);
        // console.log("/1", data.reduce((a, b) => a + b, 0)/data.length);
        // console.log("sum2", data2.reduce((a, b) => a + b, 0));
        // console.log("length2", data2.length);
        // console.log("/2", data2.reduce((a, b) => a + b, 0)/data2.length);
        this.value.push(data.reduce((a, b) => a + b, 0) / data.length);
        this.value2.push(data2.reduce((a, b) => a + b, 0) / data2.length);
      });
      this.renderChart(
        {
          labels: this.labels,
          datasets: [
            {
              label: "Data",
              borderColor: "#FC2525",
              pointBackgroundColor: "white",
              borderWidth: 1,
              pointBorderColor: "white",
              backgroundColor: this.gradient,
              data: this.value,
            },
            {
              label: "Data2",
              borderColor: "#05CBE1",
              pointBackgroundColor: "white",
              pointBorderColor: "white",
              borderWidth: 1,
              backgroundColor: this.gradient2,
              data: this.value2,
            },
          ],
        },
        { responsive: true, maintainAspectRatio: false }
      );
    });
    this.gradient = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);
    this.gradient2 = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);

    this.gradient.addColorStop(0, "rgba(255, 0,0, 0.5)");
    this.gradient.addColorStop(0.5, "rgba(255, 0, 0, 0.25)");
    this.gradient.addColorStop(1, "rgba(255, 0, 0, 0)");

    this.gradient2.addColorStop(0, "rgba(0, 231, 255, 0.9)");
    this.gradient2.addColorStop(0.5, "rgba(0, 231, 255, 0.25)");
    this.gradient2.addColorStop(1, "rgba(0, 231, 255, 0)");
  },
};
</script>
