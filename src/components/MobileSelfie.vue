<!-- <template>
  <div>
    <div v-if="firstShowSnapshots" id="app" class="web-camera-container">
      <div v-show="isCameraOpen && isLoading" class="camera-loading">
        <ul class="loader-circle">
          <li></li>
          <li></li>
          <li></li>
        </ul>
      </div>
      <div>This process requires you to provide a snapshot of yourself</div>
      <div>You have to allow your front camera to capture your snap</div>
      <div>Provide Your Snapshot</div>
      <div
        v-if="isCameraOpen"
        v-show="!isLoading"
        class="camera-box"
        :class="{ flash: isShotPhoto }"
      >
        <div class="camera-shutter" :class="{ flash: isShotPhoto }"></div>
        <video id="cam" autoplay muted playsinline>Not available</video>
        <video
          v-show="!isPhotoTaken"
          ref="camera"
          :width="450"
          :height="337.5"
          autoplay
          muted
          playsinline
        ></video>
        <video id="cam" autoplay muted playsinline>Not available</video>

        <canvas
          v-show="isPhotoTaken"
          id="photoTaken"
          ref="canvas"
          :width="450"
          :height="337.5"
        ></canvas>
      </div>

      <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
        <button type="button" class="button" @click="takePhoto">
          <img
            src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png"
          />
        </button>
      </div>

      <span v-if="isPhotoTaken && isCameraOpen" class="camera-download">
        <a
          id="downloadPhoto"
          download="my-photo.jpg"
          class="button"
          role="button"
          @click="downloadImage"
        >
          Download
        </a>
      </span>
    </div>
    <div v-if="!firstShowSnapshots" id="app" class="web-camera-container">
      <div v-show="isCameraOpen && isLoading" class="camera-loading">
        <ul class="loader-circle">
          <li></li>
          <li></li>
          <li></li>
        </ul>
      </div>
      <div>This process requires you to provide a snapshot of your ID Card</div>
      <div>You have to allow your front camera to capture your ID Card</div>
      <div>Take snapshot of your ID Card</div>
      <div
        v-if="isCameraOpen"
        v-show="!isLoading"
        class="camera-box"
        :class="{ flash: isShotPhoto }"
      >
        <div class="camera-shutter" :class="{ flash: isShotPhoto }"></div>

        <video
          v-show="!isPhotoTaken"
          ref="camera"
          :width="450"
          :height="337.5"
          autoplay
          muted
          playsinline
        ></video>

        <canvas
          v-show="isPhotoTaken"
          id="photoTaken"
          ref="canvas"
          :width="450"
          :height="337.5"
        ></canvas>
      </div>

      <div v-if="isCameraOpen && !isLoading" class="camera-shoot">
        <button type="button" class="button" @click="takePhoto">
          <img
            src="https://img.icons8.com/material-outlined/50/000000/camera--v2.png"
          />
        </button>
      </div>

      <div v-if="isPhotoTaken && isCameraOpen" class="camera-download">
        <a
          id="downloadPhoto"
          download="my-photo.jpg"
          class="button"
          role="button"
          @click="downloadImage"
        >
          Download
        </a>
      </div>
    </div>
    <div>
      <button v-if="firstShowSnapshots">
        take A picture first and then go for id capturing
      </button>
      <button
        @click="iWantToGoBacktoTakeMyPicAgain()"
        v-if="!firstShowSnapshots"
      >
        go back (y u want go back u already took photo write)
      </button>
      <button @click="gofortakingidpicture()">next</button>
      <button @click="stopCamera()">stop</button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      firstShowSnapshots: true,
      isCameraOpen: false,
      isPhotoTaken: false,
      isShotPhoto: false,
      isLoading: false,
      link: "#",
    };
  },
  created() {
    this.toggleCamera();
  },
  watch: {
    isPhotoTaken() {
      console.log("derdf");
    },
  },
  methods: {
    stopCamera() {
      console.log("i tried but idk what is the result");
    },
    gofortakingidpicture() {
      this.firstShowSnapshots = false;
    },
    iWantToGoBacktoTakeMyPicAgain() {
      this.firstShowSnapshots = true;
    },
    toggleCamera() {
      if (this.isCameraOpen) {
        this.isCameraOpen = false;
        this.isPhotoTaken = false;
        this.isShotPhoto = false;
        this.stopCameraStream();
      } else {
        this.isCameraOpen = true;
        this.createCameraElement();
      }
    },

    createCameraElement() {
      this.isCameraOpen = true;

      this.isLoading = true;

      const constraints = (window.constraints = {
        audio: false,
        video: true,
      });

      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          this.isLoading = false;
          this.$refs.camera.srcObject = stream;
        })
        .catch((error) => {
          this.isLoading = false;
          alert("May the browser didn't support or there is some errors.");
        });
    },

    stopCameraStream() {
      this.isCameraOpen = false;
      let tracks = this.$refs.camera.srcObject.getTracks();
      tracks.forEach((track) => {
        track.stop();
      });
    },

    takePhoto() {
      if (!this.isPhotoTaken) {
        this.isShotPhoto = true;

        const FLASH_TIMEOUT = 50;

        setTimeout(() => {
          this.isShotPhoto = false;
        }, FLASH_TIMEOUT);
      }

      this.isPhotoTaken = !this.isPhotoTaken;

      const context = this.$refs.canvas.getContext("2d");
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5);
    },

    downloadImage() {
      const download = document.getElementById("downloadPhoto");
      const canvas = document
        .getElementById("photoTaken")
        .toDataURL("image/jpeg")
        .replace("image/jpeg", "image/octet-stream");
      download.setAttribute("href", canvas);
    },
  },
};
</script>
<style scoped>
body {
  display: flex;
  justify-content: center;
}
.web-camera-container {
  margin-top: 2rem;
  margin-bottom: 2rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.web-camera-container .camera-button {
  margin-bottom: 2rem;
}
.web-camera-container .camera-box .camera-shutter {
  opacity: 0;
  width: 450px;
  height: 337.5px;
  background-color: #fff;
  position: absolute;
}
.web-camera-container .camera-box .camera-shutter.flash {
  opacity: 1;
}
.web-camera-container .camera-shoot {
  margin: 1rem 0;
}
.web-camera-container .camera-shoot button {
  height: 60px;
  width: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 100%;
}
.web-camera-container .camera-shoot button img {
  height: 35px;
  object-fit: cover;
}
.web-camera-container .camera-loading {
  overflow: hidden;
  height: 100%;
  position: absolute;
  width: 100%;
  min-height: 150px;
  margin: 3rem 0 0 -1.2rem;
}
.web-camera-container .camera-loading ul {
  height: 100%;
  position: absolute;
  width: 100%;
  z-index: 999999;
  margin: 0;
}
.web-camera-container .camera-loading .loader-circle {
  display: block;
  height: 14px;
  margin: 0 auto;
  top: 50%;
  left: 100%;
  transform: translateY(-50%);
  transform: translateX(-50%);
  position: absolute;
  width: 100%;
  padding: 0;
}
.web-camera-container .camera-loading .loader-circle li {
  display: block;
  float: left;
  width: 10px;
  height: 10px;
  line-height: 10px;
  padding: 0;
  position: relative;
  margin: 0 0 0 4px;
  background: #999;
  animation: preload 1s infinite;
  top: -50%;
  border-radius: 100%;
}
.web-camera-container .camera-loading .loader-circle li:nth-child(2) {
  animation-delay: 0.2s;
}
.web-camera-container .camera-loading .loader-circle li:nth-child(3) {
  animation-delay: 0.4s;
}
@keyframes preload {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.4;
  }
  100% {
    opacity: 1;
  }
}
</style> -->

<template>
  <div>
    <div v-show="snapShotComponent">
      <div class="mtd-header">Add your Snapshot</div>
      <div class="mss-detail-container">
        <b-row>
          <b-col cols="12">
            <div class="mss-headtext">
              This process requires you to provide a snapshot of yourself
            </div>
          </b-col>
        </b-row>
        <b-row>
          <b-col cols="12">
            <div class="mss-subheadtext mt-3">
              You have to allow your front camera to capture your snap
            </div>
          </b-col>
        </b-row>

        <b-row>
          <b-col cols="12">
            <div class="mss-ss-text mt-4">Provide Your Snapshot</div>
          </b-col>
        </b-row>
        <b-row>
          <b-col cols="12">
            <div class="mt-2">
              <div
                class="d-flex align-items-center justify-content-center"
                v-if="isCameraOpen"
              >
                <video
                  v-show="!isPhotoTaken"
                  id="mobileselfieCam"
                  ref="camera"
                  :width="265"
                  :height="200"
                  autoplay
                  muted
                  playsinline
                ></video>
                <canvas
                  v-show="isPhotoTaken"
                  id="photoTaken"
                  ref="canvas"
                  :width="265"
                  :height="200"
                ></canvas>
              </div>
            </div>
            <div class="mt-3 d-flex align-items-center justify-content-center">
              <button type="button" class="checkbtn" @click="takePhoto">
                <img src="../assets/icons/systemcheck/capture.svg" alt="" />
                {{ imageTakenStatus }}
              </button>
            </div>
          </b-col>
        </b-row>
      </div>
      <b-row class="m-1">
        <b-col cols="9">
          <div class="mss-nxt-note mt-5">
            Next:Take snapshot of your ID card
          </div>
        </b-col>
        <b-col cols="3">
          <div @click="goToTakeId()" class="mss-next-btn mt-5">
            Next
            <img src="../assets/mobilesvgs/nxtvector.svg" class="ml-2" alt="" />
          </div>
        </b-col>
      </b-row>
    </div>
    <div>
      <!-- ----------------------------ID CARD----------------------------- -->
      <div v-show="idShotComponent">
        <div class="mtd-header">Add your Snapshot</div>
        <div class="mss-detail-container">
          <b-row>
            <b-col cols="12">
              <div class="mss-headtext">
                This process requires you to provide a snapshot of your ID Card
              </div>
            </b-col>
          </b-row>
          <b-row>
            <b-col cols="12">
              <div class="mss-subheadtext mt-3">
                You have to allow your front camera to capture your ID Card
              </div>
            </b-col>
          </b-row>

          <b-row>
            <b-col cols="12">
              <div class="mss-ss-text mt-4">Take snapshot of your ID Card</div>
            </b-col>
          </b-row>
          <b-row>
            <b-col cols="12 ">
              <div class="mt-2">
                <div
                  class="d-flex align-items-center justify-content-center"
                  v-if="idisCameraOpen"
                >
                  <video
                    v-show="!idisPhotoTaken"
                    id="midCam"
                    ref="idcamera"
                    :width="265"
                    :height="200"
                    autoplay
                    muted
                    playsinline
                  ></video>
                  <canvas
                    v-show="idisPhotoTaken"
                    id="idphotoTaken"
                    ref="idcanvas"
                    :width="265"
                    :height="200"
                  ></canvas>
                </div>
                <div class="text-center mt-3">
                  <button type="button" class="checkbtn" @click="idtakePhoto">
                    <img src="../assets/icons/systemcheck/capture.svg" alt="" />
                    {{ idCardImageTakenStatus }}
                  </button>
                </div>
              </div>
            </b-col>
          </b-row>
        </div>
        <b-row class="m-1">
          <b-col cols="9">
            <div @click="goToSelfie()" class="mss-nxt-note mt-5 text-underline">
              Previous
            </div>
          </b-col>
          <b-col @click="sendToS3" cols="3">
            <div class="mss-next-btn mt-5">
              Next
              <img
                src="../assets/mobilesvgs/nxtvector.svg"
                class="ml-2"
                alt=""
              />
            </div>
          </b-col>
        </b-row>
      </div>
    </div>
    <div v-if="showMob">
      <b-modal id="mobilelocationmodalandroid" centered hide-footer hide-header>
        <div class="mobloc-heading d-flex align-items-center">
          Enable System Permissions
        </div>
        <!-- <div @click="$bvModal.hide('mobilelocationmodal')" class="xMarkCancel">
        <img src="../../assets/xmark.svg" alt="" />
      </div> -->
        <div class="mt-3">
          <div v-if="showsteptwo == false">
            <div class="mobloc-steptext">Please click on site settings</div>
            <div class="mt-3">
              <img src="../assets/mobilesvgs/stepone.svg" alt="" />
            </div>
            <div class="d-flex align-items-center justify-content-end mt-3">
              <b-button class="addbtn" @click="showsteptwo = true"
                >Next</b-button
              >
            </div>

            <!-- <b-button class="tp-cancelbtn">Cancel</b-button> -->
          </div>
          <div v-if="showsteptwo == true">
            <div class="mobloc-steptext">2. And Enable all permissions</div>
            <div class="text-center mt-3">
              <img src="../assets/mobilesvgs/step2.svg" alt="" />
            </div>
            <div class="d-flex align-items-center justify-content-between mt-3">
              <div class="mobloc-bcktxt" @click="showsteptwo = false">Back</div>
              <b-button
                class="addbtn"
                @click="recalModelIfNotEnabledPermissionM()"
                >Okay</b-button
              >
            </div>

            <!-- <b-button class="tp-cancelbtn">Cancel</b-button> -->
          </div>
        </div>
      </b-modal>
      <b-modal id="mobilelocationmodalios" centered hide-footer hide-header>
        <div class="mobloc-heading d-flex align-items-center">
          Enable System Permissions
        </div>
        <!-- <div @click="$bvModal.hide('mobilelocationmodal')" class="xMarkCancel">
        <img src="../../assets/xmark.svg" alt="" />
      </div> -->
        <div class="mt-3">
          <div v-if="showsteptwo == false">
            <div class="mobloc-steptext">Please click on site settings</div>
            <div class="mt-3">
              <img src="../assets/mobilesvgs/iosModela1.svg" alt="" />
            </div>
            <div class="d-flex align-items-center justify-content-end mt-3">
              <b-button class="addbtn" @click="showsteptwo = true"
                >Next</b-button
              >
            </div>

            <!-- <b-button class="tp-cancelbtn">Cancel</b-button> -->
          </div>
          <div v-if="showsteptwo == true">
            <div class="mobloc-steptext">2. And Enable all permissions</div>
            <div class="text-center mt-3">
              <img src="../assets/mobilesvgs/iosmodela2.svg" alt="" />
            </div>
            <div class="d-flex align-items-center justify-content-between mt-3">
              <div class="mobloc-bcktxt" @click="showsteptwo = false">Back</div>
              <b-button
                class="addbtn"
                @click="recalModelIfNotEnabledPermissionM()"
                >Okay</b-button
              >
            </div>

            <!-- <b-button class="tp-cancelbtn">Cancel</b-button> -->
          </div>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
// import { mapGetters } from "vuex";
// import axios from "axios";
// import { candidateImage } from "../../apiFunction";
const snapShot = {
  data() {
    return {
      osCheckforModel: "",

      showMob: false,
      showsteptwo: false,
      snapShotComponent: true,
      idShotComponent: false,
      isCameraOpen: true,
      isPhotoTaken: false,
      idisCameraOpen: true,
      idisPhotoTaken: false,
      capturedImage: null,
      capturedIdimage: null,
      idCardImageTakenStatus: "Capture",
      imageTakenStatus: "Capture",
      topicid: "",
      testid: "",
      userid: "",
    };
  },
  created() {
    this.askCameraPermission();
    this.createCameraElement();
    this.idcreateCameraElement();
    // this.topicid = this.testDetails.topicID;
    // this.testid = this.testDetails.testID;
    // this.userid = this.userID;
    let width = window.innerWidth;
    if (width > 820) {
      console.log(width);
      this.showMob = false;
    }
    if (width < 820) {
      this.showMob = true;
    }
    this.osCheckforModel = navigator.userAgentData.platform;
  },
  mounted() {},
  // computed: {
  //   ...mapGetters("sectionVUEX", {
  //     isLoadingStatus: "getIsLoadingStatus",
  //     testDetails: "getTestDetails",
  //     userID: "getUserID",
  //     userDetials: "getUserDetails",
  //     uuid: "getUUID",
  //   }),
  // },
  methods: {
    recalModelIfNotEnabledPermissionM() {
      if (this.osCheckforModel === "Android") {
        this.$bvModal.hide("mobilelocationmodalandroid");
      } else {
        this.$bvModal.hide("mobilelocationmodalios");
      }
      this.askCameraPermission();
    },
    cancel() {
      const idCardObj = document.getElementById("midCam");
      idCardObj.srcObject = null;
      const picObj = document.getElementById("mobileselfieCam");
      picObj.srcObject = null;

      window.selfieCamStream.getVideoTracks()[0].stop();
      window.idCamStream.getVideoTracks()[0].stop();
      this.$router.push(
        {
          path: "/",
          query: {
            uuid: this.uuid,
          },
        },
        () => {
          this.$router.go(0);
        }
      );
    },
    askCameraPermission() {
      navigator.mediaDevices
        .getUserMedia({
          video: true,
          audio: true,
        })
        .then(
          (res) => console.log(res),
          (err) => {
            if (this.osCheckforModel === "Android") {
              this.$bvModal.show("mobilelocationmodalandroid");
            } else {
              this.$bvModal.show("mobilelocationmodalios");
            }
            console.log(err);
          }
        );
    },
    createCameraElement() {
      const constraints = {
        audio: false,
        video: true,
      };

      navigator.mediaDevices.getUserMedia(constraints).then((stream) => {
        this.$refs.camera.srcObject = stream;
        window.selfieCamStream = stream;
      });
    },
    idcreateCameraElement() {
      const constraints = {
        audio: false,
        video: true,
      };

      navigator.mediaDevices.getUserMedia(constraints).then((stream) => {
        this.$refs.idcamera.srcObject = stream;
        window.idCamStream = stream;
      });
    },
    dataURLtoFile(dataurl, filename) {
      var arr = dataurl.split(","),
        mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]),
        n = bstr.length,
        u8arr = new Uint8Array(n);

      while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
      }

      return new File([u8arr], filename, { type: mime });
    },
    async takePhoto() {
      this.isPhotoTaken = !this.isPhotoTaken;
      if (this.isPhotoTaken) {
        this.imageTakenStatus = "Captured";
      } else {
        this.imageTakenStatus = "Capture";
      }
      const context = this.$refs.canvas.getContext("2d");
      context.drawImage(this.$refs.camera, 0, 0, 265, 230);
      var src = this.$refs.canvas.toDataURL("image/png");
      var file = this.dataURLtoFile(src, "selfie.png");
      this.capturedImage = file;
    },
    idtakePhoto() {
      this.idisPhotoTaken = !this.idisPhotoTaken;
      if (this.idisPhotoTaken) {
        this.idCardImageTakenStatus = "Captured";
      } else {
        this.idCardImageTakenStatus = "Capture";
      }
      const context = this.$refs.idcanvas.getContext("2d");
      context.drawImage(this.$refs.idcamera, 0, 0, 265, 230);
      var src = this.$refs.idcanvas.toDataURL("image/png");
      var file = this.dataURLtoFile(src, "Idpicture.png");
      this.capturedIdimage = file;
    },
    sendToS3() {
      window.selfieCamStream.getVideoTracks()[0].stop();
      window.idCamStream.getVideoTracks()[0].stop();
      var formData = new FormData();
      // let topicId = this.topicid;
      // let testID = this.testid;
      // let cuserid = this.userid;
      // formData.append("topicID", topicId);
      // formData.append("testID", testID);
      // formData.append("groupID", this.testDetails.groupID);
      // formData.append("candidateID", this.userID);
      formData.append("candidateImage", this.capturedImage);
      formData.append("candidateImageWithID", this.capturedIdimage);
      // formData.append("emailID", this.userDetials.email);
      // formData.append("attemptNo", this.testDetails.attemptNo);

      if (this.idisPhotoTaken && this.isPhotoTaken) {
        // candidateImage(formData).then(() => {
        //   window.selfieCamStream.getVideoTracks()[0].stop();
        //   window.idCamStream.getVideoTracks()[0].stop();
        //   if (this.testDetails.config.typingTest == true) {
        //     this.$router.push({
        //       path: "/TypingTest",
        //     });
        //   } else {
        //     this.$router.push({
        //       path: "/dos",
        //     });
        //   }
        // });
        console.log(formData);
      } else {
        console.log("please  capture the pictutre to continue");
      }
    },
    goToTakeId() {
      if (this.isPhotoTaken) {
        this.idShotComponent = !this.idShotComponent;
        this.snapShotComponent = !this.snapShotComponent;
      } else {
        console.log("please  capture the pictutre to continue");
      }
    },
    goToSelfie() {
      this.idShotComponent = !this.idShotComponent;
      this.snapShotComponent = !this.snapShotComponent;
      // this.isPhotoTaken = !this.isPhotoTaken;
    },
  },
};
export default snapShot;
</script>

<style scoped>
#mobileselfieCam,
#midCam {
  /* width: 100%;
  height: 100%;
  border-radius: 5px; */
  border: 5px solid gray;
}

#photoTaken,
#idphotoTaken {
  /* width: 100%;
  height: 100%;
  border-radius: 5px; */

  border: 5px solid green;
}
</style>
