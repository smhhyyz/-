<template>
  <div style="margin:12px">
    <v-icon large left style="margin-left: 6px; margin-right: 10px">
      {{ humid_icon }}
    </v-icon>
    <span style="font-size: 30px" v-text="humidity + '%'"></span>
  </div>
</template>

<script>
export default {
  name: "Humidity",
  data: () => ({
    humid_icon: "fa-tint",
    humidity: "0.0",
  }),
  mounted() {},
  methods: {
    updateData() {
      this.humidity = this.$parent.humidity;
    },
  },
};
</script>

<style>
</style>