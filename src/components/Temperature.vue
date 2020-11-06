<template>
  <div>
    <v-chip color="blue" label text-color="white" large style="margin-right:25px">
      <v-icon large left>
        {{ temp_icon }}
      </v-icon>
      <span style="font-size: 30px" v-text="temperature + '℃'"></span>
    </v-chip>
    <v-chip color="blue" label text-color="white" large>
      <v-icon large left>
        {{ humid_icon }}
      </v-icon>
      <span style="font-size: 30px" v-text="humidity + '%'"></span>
    </v-chip>
  </div>
</template>

<script>
export default {
  name: "Temperature",
  data: () => ({
    temperature: "0.0",
    humidity:"0",
    temp_iconSets: [
      "fa-thermometer-empty",
      "fa-thermometer-quarter",
      "fa-thermometer-half",
      "fa-thermometer-three-quarters",
    ],
    temp_icon: "fa-thermometer-empty",
    humid_icon:"fa-tint"
  }),
  mounted() {
    this.getTemperature();
  },
  methods: {
    getTemperature() {
      var that = this;
      that.$axios.get("/surroundings/indoor_weather").then((res) => {
        if (res) {
          res = res.data;
          if (res.code == 1) {
            console.log(res);
            that.temperature = res.data.tmp;
            that.humidity = res.data.hmd;
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

<style>
</style>