<template>
  <div class="row">
    <div class="toggle" @click="toggle">TOGGLE</div>
    <audio-recorder
      v-if="showRecorder"
      filename="ninja"
      format="wav"
      :attempts="30"
      :time="2"
      :headers="headers"
      :before-recording="callback"
      :pause-recording="callback"
      :after-recording="callback"
      :select-record="callback"
      :before-upload="callback"
      :successful-upload="callback"
      :failed-upload="callback"
      :bit-rate="192"
    />

    <audio-player :src="mp3" v-if="!showRecorder" />
  </div>
</template>

<script>
import Vue from "vue";
import AudioRecorder from "vue-audio-recorder";
Vue.use(AudioRecorder);

export default {
  name: "MyVoice",
  data() {
    return {
      mp3: "/demo/example.mp3",
      showRecorder: true,
      headers: {
        "X-Custom-Header": "some data"
      }
    };
  },
  components: {},
  methods: {
    callback(data) {
      // eslint-disable-next-line no-console
      console.log(data);
    },
    toggle(data) {
      // eslint-disable-next-line no-console
      console.log(data);
    }
  }
};
</script>
