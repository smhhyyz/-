<template>
  <div>
    <v-col>
      <v-row>
        <v-icon :color="color" x-large>{{ icon }}</v-icon>
        <span v-text="emotion"></span>
      </v-row>
      <v-row>
        <div id="charts" style="width: 400px; height: 200px"></div>
      </v-row>
    </v-col>
  </div>
</template>

<script>
export default {
  name: "EmotionDetector",
  data: () => ({
    emotion: "Detecting...",
    icon: "fa-circle-notch fa-spin",
    color: "blue",
    chart: null,
    emotionTable: ["angry", "disgust", "sad", "surprise", "neutral", "happy"],
  }),
  methods: {
    getFace() {
      var that = this;
      that.$axios.get("/emotions/getEmotion").then((res) => {
        res = res.data;
        if (res.code == "1") {
          var emotionCode = -1;
          if (res.data.emotion == "happy") {
            that.icon = "fa-laugh-beam";
            that.color = "green";
            emotionCode = 5;
            that.emotion = res.data.emotion;
          } else if (res.data.emotion == "neutral") {
            that.icon = "fa-meh";
            that.color = "blue";
            emotionCode = 4;
            that.emotion = res.data.emotion;
          } else if (res.data.emotion == "angry") {
            that.icon = "fa-angry";
            that.color = "red";
            emotionCode = 0;
            that.emotion = res.data.emotion;
          } else if (res.data.emotion == "sad") {
            that.icon = "fa-tired";
            that.color = "red";
            emotionCode = 2;
            that.emotion = res.data.emotion;
          } else if (res.data.emotion == "surprise") {
            that.icon = "fa-surprise";
            that.color = "yellow";
            emotionCode = 3;
            that.emotion = res.data.emotion;
          } else if (res.data.emotion == "disgust") {
            that.icon = "fa-frown";
            that.color = "red";
            emotionCode = 1;
            that.emotion = res.data.emotion;
          } else {
            console.log(res);
            console.log("unexcepted emotion:\t" + that.emotion);
          }
          if (emotionCode >= 0 && emotionCode <= 5) {
            var time = new Date().toLocaleTimeString().replace(/^\D*/, "");
            that.updateChart(time, emotionCode);
          }
        }
        setTimeout(function () {
          that.getFace();
        }, 500);
      });
    },
    updateChart(x, y) {
      var opt = this.chart.getOption();
      var data = opt.series[0].data;
      if (opt.xAxis[0].data.length > 10) {
        opt.xAxis[0].data.shift();
      }
      opt.xAxis[0].data.push(x);
      if (data.length > 10) {
        data.shift();
      }
      data.push(y);

      this.chart.setOption(opt);
    },
    drawLine() {
      var that = this;
      this.chart = this.$echarts.init(document.getElementById("charts"));
      // 绘制图表
      this.chart.setOption({
        xAxis: {
          type: "category",
          data: [
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
            "00:00",
          ],
          axisLabel:{
            show: true,
            color: "white",
          }
        },
        yAxis: {
          type: "value",
          max: 5,
          min: 0,
          axisLabel: {
            show: true,
            interval: "auto", // {number}
            color: "white",
            align: "right",
            fontSize: 8,
            formatter: function (value, index) {
              return that.emotionTable[value];
            }, // Template formatter!
          },
        },
        series: [
          {
            data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            type: "line",
          },
        ],
      });
    },
  },
  mounted() {
    this.getFace();
    this.drawLine();
  },
};
</script>

<style>
</style>