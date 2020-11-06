<template>
  <div>
    <v-icon :color="color" x-large>{{ icon }}</v-icon>
    <p :v-text="emotion"></p>
  </div>
</template>

<script>
export default {
  name: "EmotionDetector",
  data: () => ({
    emotion: "Detecting...",
    icon: "fa-circle-notch fa-spin",
    color: "blue",
  }),
  methods: {
    getFace(){
      var that = this
      that.$axios.get("/faceRecognize/getEmotion")
      .then((res) => {
        res = res.data
          if (res.code == "1") {
            that.emotion = res.data.emotion;
            if (that.emotion == "happy") {
              that.icon = "fa-laugh-beam";
              that.color = "green";
            } else if (that.emotion == "neutral") {
              that.icon = "fa-meh";
              that.color = "blue";
            } else if (that.emotion == "angry") {
              that.icon = "fa-angry";
              that.color = "red";
            } else if (that.emotion == "sad") {
              that.icon = "fa-tired";
              that.color = "red";
            } else if (that.emotion == "surprise") {
              that.icon = "fa-surprise";
              that.color = "yellow";
            } else if(that.emotion == "disgust"){
              that.icon = "fa-frown";
              that.color = "red";
            }
          }
          setTimeout(that.getFace(), 500);
        });
    },
  },
  mounted(){
    this.getFace();
  },
};
</script>

<style>
</style>