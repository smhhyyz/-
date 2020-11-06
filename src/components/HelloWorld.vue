<template>
  <v-container>
    <v-row class="text-center">
      <v-col>
        <EmotionDetector />
      </v-col>
      <v-col>
        <FlipClock />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import FlipClock from "kuan-vue-flip-clock";
import EmotionDetector from "./EmotionDetector";
export default {
  name: "HelloWorld",
  components: {
    FlipClock,
    EmotionDetector,
  },
  data: () => ({}),
};
</script>
