<template>
  <div class="height: 20px">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from "chart.js";

export default {
  name: "StackChart",
  data: function () {
    this.chart = null; // important https://stackoverflow.com/questions/68602389/maximum-call-stack-error-when-attempting-to-update-chart-in-vue-js
    return {};
  },
  props: {
    chartData: Array,
    duration: Number,
  },

  methods: {
    formatData() {
      let labels = [];
      for (let x = 0; x < this.duration; x += 1) {
        let total = 0;

        this.chartData.forEach((entry) => {
          if (x >= entry.start && x < entry.start + entry.duration) {
            total += 1;
          }
        });
        labels.push(total);
      }

      return labels;
    },
    formatLabels() {
      let labels = [];
      for (let x = 0; x < this.duration; x += 1) {
        labels.push(x);
      }

      return labels;
    },
  },
  watch: {
    duration() {
      this.chart.data.labels = this.formatLabels();
      this.chart.update();
    },
    chartData: {
      deep: true, // needed to look into the array
      handler() {
        this.chart.data.datasets[0].data = this.formatData();
        this.chart.update();
      },
    },
  },
  mounted() {
    Chart.register(...registerables);
    this.chart = new Chart(this.$refs.canvas.getContext("2d"), {
      type: "bar",
      data: {
        labels: this.formatLabels(),
        datasets: [
          {
            data: this.formatData(),
          },
        ],
      },
      options: {
        scales: {
          xAxis: {
            grid: {
              display: false,
            },
          },
          yAxis: {
            display: false,
          },
        },
        maintainAspectRatio: false,
        responsive: true,
        plugins: {
          legend: {
            display: false,
          },
          title: {
            display: false,
          },
        },
      },
    });
  },
};
</script>

<style scoped></style>
