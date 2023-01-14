<template>
  <div>
    <client-only>
      <ApexCharts
        :options="chartOptions()"
        :series="series"
        :height="460"
        :width="1000"
        chart-id="vuechart-example"
      />
    </client-only>
  </div>
</template>

<script>
export default {
  name: "donutPage",
  data() {
    return {
      posts: "",
      series: [450, 300],
    };
  },
  methods: {
    async getkeys() {
      for (const [key, value] of Object.entries(this.posts.result)) {
        // console.log(`key: ${key}: value : ${value.total_activity} , ${value.total_task}`);
        this.options.xaxis.categories.push(key);
        this.series[0].data.push(value.total_activity);
        this.series[1].data.push(value.total_task);
      }
    },
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
                  label: "تمام تسک ها",
                  formatter: (w) => {
                    return w.globals.seriesTotals.reduce((a, b) => {
                      return a + b;
                    }, 0);
                  },
                },
              },
            },
          },
        },
        dataLabels: {
          show: true,

          formatter: (val, opts) => {
            return opts.w.config.series[opts.seriesIndex];
          },
        },
      };
    },
  },
  async fetch() {
    const {
      data: { result },
    } = await this.$axios.$get(
      "https://ws.datisint.com/api/v2/staff/report/getReport/12?user_id=1077&start_date=0&end_date=1857617000&type=creatorl",
      {
        headers: {
          Authorization:
            "Bearer 30920|xutaXgwfRtKJh80VvMIwHWVcbFeAQJo5fv4zI0Ey",
          "Content-Type": "application/json",
        },
      }
    );
    this.posts = result;
  },

  mounted() {
    // await this.getkeys();
  },
};
</script>
