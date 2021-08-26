<template>
  <v-card class="mx-auto" width="100%">
    <v-card-title style="background-color: mediumseagreen">
      <v-icon large left> mdi-apps </v-icon>
      <span class="text-h6 font-weight-light">Assignment</span>
    </v-card-title>
    <v-card-text>
      <v-chip-group active-class="deep-purple accent-4 white--text" column>
        <v-chip @click="List()" :class="css">
          <v-icon left>mdi-view-list</v-icon>
          List
        </v-chip>
        <v-chip @click="Calendar()">
          <v-icon left>mdi-calendar-today</v-icon>
          Calendar
        </v-chip>
        <v-chip @click="Graph()">
          <v-icon left>mdi-chart-line</v-icon>
          Graph
        </v-chip>
      </v-chip-group>
    </v-card-text>
    <div v-if="checkList">
      <TableList />
    </div>
    <div v-if="checkCalendar">
      <Calendar />
    </div>
    <div v-if="checkGraph">
      <v-card-text class="pa-0 ma-0">
        <div class="font-weight-bold ml-4">List Graph</div>
      </v-card-text>
      <AreaChart />
      <BarChart />
    </div>
  </v-card>
</template>
<script>
import AreaChart from "../components/AreaChart.vue";
import BarChart from "../components/BarChart.vue";
import Calendar from "../components/Calendar.vue";
import TableList from "../components/TableList.vue";
export default {
  name: "Home",
  components: {
    AreaChart,
    BarChart,
    Calendar,
    TableList,
  },
  data: () => ({
    css: "v-chip deep-purple accent-4 white--text v-chip--active v-chip--clickable v-chip--no-color theme--light v-size--default",
    search: "",
    checkList: true,
    checkCalendar: false,
    checkGraph: false,
  }),
  methods: {
    List() {
      this.checkList = true;
      this.checkCalendar = false;
      this.checkGraph = false;
    },
    Calendar() {
      this.css =
        "v-chip v-chip--clickable v-chip--no-color theme--light v-size--default";
      this.checkList = false;
      this.checkCalendar = true;
      this.checkGraph = false;
    },
    Graph() {
      this.css =
        "v-chip v-chip--clickable v-chip--no-color theme--light v-size--default";
      this.checkList = false;
      this.checkCalendar = false;
      this.checkGraph = true;
    },
  },
};
</script>

<style></style>
