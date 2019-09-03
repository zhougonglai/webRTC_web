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
    <v-btn text @click="takePhoto">
      <span class="mr-2">takePhoto</span>
    </v-btn>
    <v-btn text @click="downLoad">
      <span class="mr-2">downLoad</span>
    </v-btn>

    <canvas ref="photoGraph"></canvas>
    <div class="output">
      <img ref="photo" alt="The screen capture will appear in this box." />
    </div>
  </div>
</template>
<script>
export default {
  name: "home",
  data: () => ({
    mediaSteam: "",
    mediaRecorder: "",
    videoTracks: "",
    buffer: []
  }),
  methods: {
    getUserMedia() {
      navigator.mediaDevices
        .getUserMedia({
          audio: false,
          video: {
            width: 1280,
            height: 720,
            frameRate: 15
          }
        })
        .then(mediaSteam => {
          this.mediaSteam = mediaSteam;
          this.mediaRecorder = new MediaRecorder(mediaSteam, {
            mimeType: "video/webm;codecs=vp8"
          });
          console.log(this.mediaRecorder);
          if ("srcObject" in this.$refs.video) {
            // this.$refs.video.srcObject = mediaSteam;
            this.$refs.video.srcObject = this.mediaRecorder.stream;
          } else {
            this.$refs.video.src = window.URL.createObjectURL(stream);
          }
          this.mediaRecorder.ondataavailable = e => {
            if (e && e.data && e.data.size > 0) {
              this.buffer.push(e.data);
            }
          };
          this.mediaRecorder.start(10);
          this.$refs.video.onloadedmetadata = () => {
            this.$refs.video.play();
          };
        });
    },
    takePhoto() {
      const context = this.$refs.photoGraph.getContext("2d");
      context.drawImage(this.$refs.video, 0, 0, 300, 150);
    },
    downLoad() {
      const blob = new Blob(this.buffer, { type: "video/warm" });
      const url = URL.createObjectURL(blob);
    },
    stopSteam() {
      console.log(this.mediaRecorder.state);
      this.mediaRecorder.stop();
      // this.$refs.video.pause();
      console.log(this.mediaRecorder.state);
    }
  },
  created() {}
};
</script>
