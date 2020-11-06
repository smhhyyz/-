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
        <v-row class="text-center">
      <v-col>
        <Temperature/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import FlipClock from "kuan-vue-flip-clock";
import EmotionDetector from "./EmotionDetector";
import Temperature from "./Temperature"
export default {
  name: "HelloWorld",
  components: {
    FlipClock,
    EmotionDetector,
    Temperature
  },
  data: () => ({}),
};
</script>
