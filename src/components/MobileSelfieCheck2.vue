<template>
  <div>
    <div class="panel">
      <button @click="switchCamera(user)" id="switchFrontBtn">
        Front Camera
      </button>
      <button @click="switchCamera(env)" id="switchBackBtn">Back Camera</button>
      <button @click="takeSnap(event)" id="snapBtn">Snap</button>
    </div>
    <div style="width: 100%">
      <!-- add autoplay muted playsinline for iOS -->
      <video id="cam" autoplay muted playsinline>Not available</video>
      <canvas id="canvas" style="display: none"></canvas>
      <img id="photo" alt="The screen capture will appear in this box." />
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      env: "user",
      user: "user",
      mediaStream: null,
      constraints: {
        audio: false,
        video: {
          width: { ideal: 640 },
          height: { ideal: 480 },
          facingMode: "environment",
        },
      },
    };
  },
  mounted() {
    this.switchCamera("user");
  },
  methods: {
    async getMediaStream(constraints) {
      try {
        this.mediaStream = await navigator.mediaDevices.getUserMedia(
          constraints
        );
        let video = document.getElementById("cam");
        video.srcObject = this.mediaStream;
        video.onloadedmetadata = (event) => {
          video.play();
        };
      } catch (err) {
        console.error(err.message);
      }
    },

    async switchCamera(cameraMode) {
      try {
        // stop the current video stream
        if (this.mediaStream != null && this.mediaStream.active) {
          var tracks = this.mediaStream.getVideoTracks();
          tracks.forEach((track) => {
            track.stop();
          });
        }

        // set the video source to null
        document.getElementById("cam").srcObject = null;

        // change "facingMode"
        this.constraints.video.facingMode = cameraMode;

        // get new media stream
        await this.getMediaStream(this.constraints);
      } catch (err) {
        console.error(err.message);
      }
    },

    takePicture() {
      let canvas = document.getElementById("canvas");
      let video = document.getElementById("cam");
      let photo = document.getElementById("photo");
      let context = canvas.getContext("2d");

      const height = video.videoHeight;
      const width = video.videoWidth;

      if (width && height) {
        canvas.width = width;
        canvas.height = height;
        context.drawImage(video, 0, 0, width, height);
        var data = canvas.toDataURL("image/png");
        photo.setAttribute("src", data);
      } else {
        clearphoto();
      }
    },

    clearPhoto() {
      let canvas = document.getElementById("canvas");
      console.log(canvas.getContext);
      let photo = document.getElementById("photo");
      let context = canvas.getContext("2d");

      context.fillStyle = "#AAA";
      context.fillRect(0, 0, canvas.width, canvas.height);
      var data = canvas.toDataURL("image/png");
      photo.setAttribute("src", data);
    },

    // document.getElementById('switchFrontBtn').onclick = (event) => {
    //   switchCamera("user");
    // }

    // document.getElementById('switchBackBtn').onclick = (event) => {
    //   switchCamera("environment");
    // }
    // document.getElementById('snapBtn').onclick = (event) => {
    //   takePicture();
    //   event.preventDefault();
    // }
    takeSnap(event) {
      console.log(event);
      this.takePicture();
    },
  },
};
</script>
<style scoped>
.panel {
  padding-bottom: 10px;
}

#cam {
  border: 1px;
  border-color: black;
  border-style: solid;
}

#photo {
  border: 1px;
  border-color: black;
  border-style: dashed;
}
</style>
