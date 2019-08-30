<template>
  <div class="home">
    <h1 class="title">RealTime communication with WebRTC</h1>
    <video autoplay playsinline ref="video" />
    <v-btn text @click="getUserMedia">
      <span class="mr-2">star</span>
    </v-btn>
    <v-btn text @click="stopSteam">
      <span class="mr-2">stop</span>
    </v-btn>
  </div>
</template>
<script>
export default {
  name: "home",
  data: () => ({
    mediaSteam: "",
    videoTracks: ""
  }),
  methods: {
    getUserMedia() {
      navigator.mediaDevices
        .getUserMedia({
          audio: true,
          video: true
        })
        .then(mediaSteam => {
          this.mediaSteam = mediaSteam;
          this.videoTracks = mediaSteam.getVideoTracks();
          if ("srcObject" in this.$refs.video) {
            this.$refs.video.srcObject = stream;
          } else {
            this.$refs.video.src = window.URL.createObjectURL(stream);
          }
          this.$refs.video.onloadedmetadata = function(e) {
            this.$refs.video.play();
          };
        });
    },
    stopSteam() {
      this.mediaSteam.stop();
    }
  },
  created() {}
};
</script>
