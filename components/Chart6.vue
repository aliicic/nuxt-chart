<template>
  <div>
    <p style="text-align: center">کار های ارجاع شده به من </p>
    <client-only>
      <ApexCharts
        :options="options"
        :series="series"
        :height="460"
        :width="600"
        chart-id="lineChart"
      />
    </client-only>
  </div>
</template>

<script>
export default {
  name: "LandingPage",
  data() {
    return {
      posts: "",
      options: {
        chart: {
          toolbar: { show: false },
          id: "vuechart-example",
          type: "bar",
          stacked: true,
        },
        xaxis: {
          categories: [],
        },
        tooltip: {
          theme: "light",
          fillSeriesColor: false,
        },
      },
      series: [
        {
          name: "منقضی شده",
          data: [],
        },
        {
          name: "درحال انجام",
          data: [],
        },
        {
          name: "انجام شده",
          data: [],
        },
      ],
    };
  },
  methods: {
    getKeyValues() {
      for (const [key, value] of Object.entries(this.posts.result)) {
        // console.log(`key: ${key}: value : ${value.total_activity} , ${value.total_task}`);
        this.options.xaxis.categories.push(key);
        this.series[0].data.push(Math.round(value.close_not_pass));
        this.series[1].data.push(Math.round(value.open_not_pass));
        this.series[2].data.push(Math.round(value.close_pass));
      }
    },
  },
  async fetch() {
    const {
      data: { result },
    } = await this.$axios.$get(
      "https://ws.datisint.com/api/v2/staff/report/getReport/22?user_id=1077&start_date=0&end_date=1857617000&type=user",
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
    this.getKeyValues();
  },
};
</script>
