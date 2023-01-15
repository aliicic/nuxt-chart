<template>
  <div>
    <p style="text-align:center">مجموع ساعات کاری ماه</p> 
    <client-only>
      <div

      >
        <ApexCharts
  
        :options="chartOptions()"
        :series="series"
        :height="460"
        :width="600"
        chart-id="vuechart-example"
      />
      <p class="sub-title" style="text-align: center">  متوسط عملکرد در هر هفته {{ middleTotal }}  ساعت </p>
      </div>
   
    </client-only>
  </div>
</template>

<script>
export default {
  name: "donutPage",
  data() {
    return {
      total: 0,
      series: [],
    };
  },
  methods: {
    chartOptions() {
      return {
        labels: ["تعداد وظیفه ها", "تعداد فعالیت ها"],
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
                  show: true,
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
  computed: {
    middleTotal() {
      return Math.round(this.total / 4);
    },
  },
  async fetch() {
    const {
      data: { result },
    } = await this.$axios.$get(
      "https://ws.datisint.com/api/v2/staff/report/getReport/12?user_id=1077&start_date=0&end_date=1857617000&type=creator",
      {
        headers: {
          Authorization:
            "Bearer 30920|xutaXgwfRtKJh80VvMIwHWVcbFeAQJo5fv4zI0Ey",
          "Content-Type": "application/json",
        },
      }
    );
    this.total = Math.round(result.total);
    this.series = [result.total_activity_count, result.total_task_count];
  },
};
</script>
<style>

.apexcharts-canvas{
  margin: 0 auto;
  background: rgb(242, 242, 242);
}
.sub-title{
  background: rgb(242, 242, 242);
  margin-top: 0;
}
</style>