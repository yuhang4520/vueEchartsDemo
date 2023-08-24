<!--
 描述: 水球图
 作者: Jack Chen
 日期: 2020-05-03
-->

<template>
  <div class="wrap-container sn-container">
    <div class="sn-content">
      <div class="sn-title">水球图</div>
      <div class="sn-body">
        <div class="wrap-container">
          <div class="chartsdom" id="chart_polo"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "waterPoloCom",
  data() {
    return {
      option: null,
    };
  },
  mounted() {
    this.getEchart();
  },
  methods: {
    getEchart() {
      let myChart = echarts.init(document.getElementById("chart_polo"));
      this.option = {
        series: [
          {
            type: "liquidFill",
            data: [0.45],
            radius: "70%",
            color: ["#00b9f5"],
            backgroundStyle: {
              color: "rgba(0, 0, 0, 0.5)",
              borderColor: "#007bff",
              borderWidth: 3,
              shadowColor: "rgba(0, 123, 225, 0.4)",
              shadowBlur: 20,
            },
            outline: {
              show: false,
            },
          },
        ],
      };

      myChart.setOption(this.option, true);

      window.addEventListener("resize", () => {
        myChart.resize();
      });
    },
  },
  beforeDestroy() {},
};
</script>

<style lang="scss" scoped>
.sn-container {
  width: 100%;
  height: 300px;
  position: relative;
  margin-bottom: 0.1875rem;

  .chartsdom {
    width: 100%;
    height: 100%;
  }
}
</style>
