<template>
  <v-app>
    <v-main>
      <v-tabs v-model="tabs" centered>
        <v-tab>Main Page</v-tab>
        <v-tab>Emotion variation</v-tab>
        <v-tab>Environment variation</v-tab>
      </v-tabs>
      <v-tabs-items v-model="tabs">
        <!-- 主页 -->
        <v-tab-item>
          <v-card flat style="width: 800px; height: 480px">
            <v-row>
              <FlipClock style="margin-top:50px;margin-bottom:50px" class="mx-auto" />
            </v-row>
            <v-row>
              <v-col >
                <EmotionDetector class="xs-6 sm-6 md-6"
                  @updateChart="updateChart"
                  @workingTime="updateWorkingTime"
                  ref="emotion"
                />
              </v-col>
              <v-col>
                <v-row>
                  <v-card elevation="3" outlined  style="width:93%">
                    <v-row>
                      <Temperature style="width:40%" ref="temperature" />
                      <v-divider vertical></v-divider>
                      <Humidity  style="width:40%;margin-left:20px" ref="humidity" />
                    </v-row>
                  </v-card>
                </v-row>
                <v-row>
                  <RelaxReminder style="width:93%" ref="reminder" />
                </v-row>
              </v-col>
            </v-row>
          </v-card>
        </v-tab-item>

        <!-- 心情变化表格页 -->
        <v-tab-item :eager="true" style="width: 800px; height: 480px">
          <v-card flat>
            <v-col>
              <EmotionChart ref="chart" />
            </v-col>
          </v-card>
        </v-tab-item>

        <!-- 环境变化表格页 -->
        <v-tab-item :eager="true" style="width: 800px; height: 480px">
          <v-card flat> </v-card>
        </v-tab-item>
      </v-tabs-items>
    </v-main>
  </v-app>
</template>

<script>
import FlipClock from "kuan-vue-flip-clock";
import EmotionDetector from "./components/EmotionDetector";
import Temperature from "./components/Temperature";
import Humidity from "./components/Humidity";
import EmotionChart from "./components/Chart";
import RelaxReminder from "./components/RelaxReminder";
export default {
  name: "App",
  components: {
    FlipClock,
    EmotionDetector,
    Temperature,
    EmotionChart,
    Humidity,
    RelaxReminder,
  },
  data: () => ({
    tabs: null,
    temperature: "0.0",
    humidity: "0.0",
  }),
  methods: {
    updateChart(x, y) {
      this.$refs.chart.updateChart(x, y);
    },
    updateWorkingTime(date) {
      this.$refs.reminder.formatDate(date);
    },
    getTemperature() {
      var that = this;
      that.$axios.get("/surroundings/indoor_weather").then((res) => {
        if (res) {
          res = res.data;
          if (res.code == 1) {
            console.log(res);
            that.temperature = res.data.tmp;
            that.humidity = res.data.hmd;
            that.$refs.humidity.updateData();
            that.$refs.temperature.updateData();
          }
        }
        setTimeout(function () {
          that.getTemperature();
        }, 25000);
      });
    },
  },
};
</script>
