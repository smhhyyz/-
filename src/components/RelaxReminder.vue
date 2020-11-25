<template>
  <div>
    <v-card elevation="3" outlined>
      <v-card-text>
        <p
          v-text="'Your are working ' + existsTime + ' continuously'"
          style="margin-right: 15px"
          v-show="init"
        ></p>
        <p
          v-text="remindText"
          style="margin-right: 15px"
          v-show="needRemind"
        ></p>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "RelaxReminder",
  props: ["parentThreshouldTime"],
  data: () => ({
    init:false,
    existsTime: "0",
    remindText: "Time to Relax !!!",
    needRemind: false,
    threshouldTime: 30,
    remindGap: 30,
    notRelax: 0,
  }),
  watch: {
    parentThreshouldTime: function () {
        var hour = this.parentThreshouldTime.match(/\d+(?=:)/)
        var minute = this.parentThreshouldTime.match(/(?<=:)\d+/)
        this.threshouldTime = parseInt(hour[0])*60 + parseInt(minute[0]);
    },
  },
  methods: {
    formatDate(date) {
      if (date == 0) {
        this.existsTime = "0";
        this.notRelax = 0;
        this.needRemind = false;
        this.init = false;
        return;
      }
      var nowDate = Date.parse(new Date()) / 1000;
      var second = nowDate - date;
      var hour = Math.floor(second / 3600);
      second = second % 3600;
      var minute = Math.floor(second / 60);
      second = second % 60;
      this.existsTime =
        hour > 9 ? hour.toString() + " : " : "0" + hour.toString() + " : ";
      this.existsTime +=
        minute > 9
          ? minute.toString() + " : "
          : "0" + minute.toString() + " : ";
      this.existsTime +=
        second > 9 ? second.toString() : "0" + second.toString();
      this.init = true;

      if (minute + hour * 60 >= this.threshouldTime) {
        if (this.notRelaxYet()) {
          this.remind();
        }
      }
    },
    notRelaxYet() {
      var bl = this.notRelax % this.remindGap == 0;
      this.notRelax += 1;
      return bl;
    },
    remind() {
      this.needRemind = true;
    },
  },
};
</script>

<style>
</style>