<template>
  <div>
    <client-only>
      <div>
        <p style="text-align: center">کار های ارجاع شده توسط من</p>
        <ApexCharts
          :options="chartOptions()"
          :series="series"
          :height="460"
          :width="600"
          chart-id="vuechart-example"
        />
      </div>
    </client-only>
  </div>
</template>

<script>
export default {

  data() {
    return {
      total: 0,
      series: [],
    };
  },
  methods: {
    chartOptions() {
      return {
        labels: ["تکمیل شده", "در حال انجام", "منقضی شده"],
        chart: {
          toolbar: { show: false },
          id: "vuechart-example",
          type: "donut",
          stacked: true,
        },
        xaxis: {
          categories: [],
        },
        tooltip: {
          theme: "light",
          fillSeriesColor: false,
        },
        plotOptions: {
          pie: {
            donut: {
              labels: {
                show: true,
                name: {
                  fontSize: "18px",
                },
                value: {
                  fontSize: "16px",
                  color: "#636a71",
                  offsetY: 11,
                },
                total: {
                  show: false,
                  enable: true,
                  label: "تمام عملکرد برحسب ساعت",
                  formatter: (w) => {
                    // return w.globals.seriesTotals.reduce((a, b) => {
                    //   return a + b;
                    // }, 0);
                    return this.total;
                  },
                },
              },
            },
          },
        },
        dataLabels: {
          show: true,

          // formatter: (val, opts) => {
          //   return opts.w.config.series[opts.seriesIndex];
          // },
        },
      };
    },
  },
  // computed: {
  //   middleTotal() {
  //     return Math.round(this.total / 4);
  //   },
  // },
  async fetch() {
    const {
      data: { result },
    } = await this.$axios.$get(
      "https://ws.datisint.com/api/v2/staff/report/getReport/22?user_id=1077&start_date=0&end_date=1857617000&type=creator",
      {
        headers: {
          Authorization:
            "Bearer 30920|xutaXgwfRtKJh80VvMIwHWVcbFeAQJo5fv4zI0Ey",
          "Content-Type": "application/json",
        },
      }
    );
    //   this.total = Math.round(result.total);
    this.series = [
      result.close_pass_count,
      result.open_not_pass_count,
      result.close_not_pass_count,
    ];
  },
};
</script>
<style>
.apexcharts-canvas {
  margin: 0 auto;
  background: rgb(242, 242, 242);
}
</style>
