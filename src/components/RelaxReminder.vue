<template>
  <div>
    <v-card elevation="3" outlined>
      <v-card-text>
        <p
          v-text="'Your are working ' + existsTime + ' continuously'"
          style="margin-right: 15px"
        ></p>
        <p v-text="remindText" style="margin-right: 15px" v-show="isRemind"></p>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "RelaxReminder",
  data: () => ({
    existsTime: "0",
    remindText: "",
    isRemind: false,
  }),
  methods: {
    formatDate(date) {
      if (date == 0) {
          this.existsTime = "0";
          this.isRemind = false;
      }
      var nowDate = Date.parse(new Date()) / 1000;
      var second = nowDate - date;
      var hour = Math.floor(second / 3600);
      second = second % 3600;
      var minute = Math.floor(second / 60);
      second = second % 60;
      this.existsTime = hour > 9 ? hour.toString()+" : " : "0" + hour.toString()+" : ";
      this.existsTime +=
        minute > 9 ? minute.toString()+" : " : "0" + minute.toString()+" : ";
      this.existsTime +=
        second > 9 ? second.toString() : "0" + second.toString();
      if (hour > 1) {
        this.isRemind = true;
      } else {
        this.isRemind = false;
      }
    },
  },
};
</script>

<style>
</style>