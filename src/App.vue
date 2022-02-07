<template>
  <div>
    <div style="margin-top: 50px">
      <line-chart
        label="Positive test results"
        :labels="reversedArrDates"
        :chart-data="reversedDatasetsPositive"
        :options="options"
        :color="colorToPositive"
      />
    </div>
    <div style="margin-top: 50px">
      <line-chart
        label="Negative test results"
        :labels="reversedArrDates"
        :chart-data="reversedDatasetsNegative"
        :options="options"
        :color="colorToNegative"
      />
    </div>
    <div style="margin-top: 50px">
      <line-chart
        label="Death"
        :labels="reversedArrDates"
        :chart-data="reversedDatasetsDeath"
        :options="options"
        :color="colorToDeath"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import LineChart from "./components/LineChart";
export default {
  name: "App",
  components: {
    LineChart,
  },
  data() {
    return {
      arrDates: [],
      datasetsPositive: [],
      colorToPositive: "#f87979",
      datasetsNegative: [],
      colorToNegative: "#d3e298",
      datasetsDeath: [],
      colorToDeath: "#353531",
      options: { responsive: true, maintainAspectRatio: false },
    };
  },
  methods: {
    formatDate(data) {
      let formatDate = data.toString().split("");
      formatDate.splice(4, 0, "-");
      formatDate.splice(7, 0, "-");
      return formatDate.join("");
    },
  },
  computed: {
    reversedArrDates() {
      return this.arrDates.slice().reverse();
    },
    reversedDatasetsPositive() {
      return this.datasetsPositive.slice().reverse();
    },
    reversedDatasetsNegative() {
      return this.datasetsNegative.slice().reverse();
    },
    reversedDatasetsDeath() {
      return this.datasetsDeath.slice().reverse();
    },
  },
  mounted() {
    axios.get("https://api.covidtracking.com/v1/us/daily.json").then((r) =>
      r.data.forEach((el) => {
        this.arrDates.push(this.formatDate(el.date));
        this.datasetsPositive.push(el.positive);
        this.datasetsNegative.push(el.negative);
        this.datasetsDeath.push(el.death);
      })
    );
  },
};
</script>