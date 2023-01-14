<template>
  <div>
    <client-only>
      <ApexCharts
        :options="options"
        :series="series"
        :height="460"
        :width="1000"
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
          name: "فعالیت ها",
          data: [],
        },
        {
          name: "وظیفه ها",
          data: [],
        },
      ],

      result: {
        result: {
          " ۷ اردیبهشت": {
            total_task: 0,
            total_activity: 0,
          },
          " ۸ خرداد": {
            total_task: 0,
            total_activity: 2.5,
          },
          "۱۱ خرداد": {
            total_task: 0,
            total_activity: 9.833333333333332,
          },
          "۱۲ خرداد": {
            total_task: 0,
            total_activity: 12.500000000000002,
          },
          "۱۳ خرداد": {
            total_task: 0,
            total_activity: 2.3333333333333335,
          },
          "۱۴ خرداد": {
            total_task: 0,
            total_activity: 4.666666666666667,
          },
          "۱۶ خرداد": {
            total_task: 0,
            total_activity: 2.3333333333333335,
          },
          "۱۷ خرداد": {
            total_task: 0,
            total_activity: 8,
          },
          "۱۸ خرداد": {
            total_task: 0,
            total_activity: 1.1666666666666667,
          },
          "۱۹ خرداد": {
            total_task: 0,
            total_activity: 1.1666666666666667,
          },
          "۲۹ خرداد": {
            total_task: 0,
            total_activity: 1.5,
          },
          "۳۰ خرداد": {
            total_task: 0,
            total_activity: 1.1666666666666667,
          },
          "۳۱ خرداد": {
            total_task: 2.3333333333333335,
            total_activity: 2.166666666666667,
          },
          "۱۲ تیر": {
            total_task: 0.16666666666666666,
            total_activity: 0,
          },
          "۲۵ مرداد": {
            total_task: 0,
            total_activity: 1,
          },
          "۳۱ شهریور": {
            total_task: 0,
            total_activity: 0,
          },
          "۱۸ مهر": {
            total_task: 0,
            total_activity: 0,
          },
          " ۲ آبان": {
            total_task: 0,
            total_activity: 1.1666666666666667,
          },
          "۲۹ آذر": {
            total_task: 2,
            total_activity: 0,
          },
          " ۴ دی": {
            total_task: 0,
            total_activity: 0,
          },
          " ۷ دی": {
            total_task: 0,
            total_activity: 0.16666666666666666,
          },
          "۱۱ دی": {
            total_task: 0,
            total_activity: 0,
          },
          "۱۲ دی": {
            total_task: 0,
            total_activity: 0,
          },
          "۱۸ دی": {
            total_task: 0,
            total_activity: 0,
          },
        },
        total: 56.16666666666666,
        total_activity_count: 39,
        total_task_count: 36,
      },
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
  },
  async fetch(){
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
  async mounted() {
    await this.getkeys();
  },
};
</script>
