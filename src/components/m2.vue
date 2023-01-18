
<template>
  <div>
    <!-- <video id="mobileselfieCam" autoplay muted playsinline>Not available</video> -->

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
              <div class="d-flex align-items-center justify-content-center">
                <video
                  id="mobileselfieCam"
                  ref="camera"
                  :width="265"
                  :height="200"
                  autoplay
                  muted
                  playsinline
                ></video>
                <!-- <video id="mobileselfieCam" autoplay muted playsinline>
                  Not available
                </video> -->

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
              <button type="button" class="checkbtn">
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
          <div class="mss-next-btn mt-5">
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
                    id="mobileselfieCam"
                    ref="idcamera"
                    :width="265"
                    :height="200"
                    autoplay
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
                  <button type="button" class="checkbtn">
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
            <div class="mss-nxt-note mt-5 text-underline">Previous</div>
          </b-col>
          <b-col cols="3">
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
              <b-button class="addbtn">Okay</b-button>
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
              <b-button class="addbtn">Okay</b-button>
            </div>

            <!-- <b-button class="tp-cancelbtn">Cancel</b-button> -->
          </div>
        </div>
      </b-modal>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      showMob: true,
      idShotComponent: false,
      isCameraOpen: false,
      snapShotComponent: true,
      isPhotoTaken: false,
      idisCameraOpen: false,
      idisPhotoTaken: false,
      idCardImageTakenStatus: "",
      imageTakenStatus: "",
      showsteptwo: false,
      env: "user",
      user: "user",
      mediaStream: null,
      constraints: {
        audio: false,
        video: {
          width: { ideal: 640 },
          height: { ideal: 480 },
          facingMode: "user",
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
        let video = document.getElementById("mobileselfieCam");
        video.srcObject = this.mediaStream;
        video.onloadedmetadata = (event) => {
          console.log(event);
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
        document.getElementById("mobileselfieCam").srcObject = null;

        // change "facingMode"
        this.constraints.video.facingMode = cameraMode;

        // get new media stream
        await this.getMediaStream(this.constraints);
      } catch (err) {
        console.error(err.message);
      }
    },
  },
};
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
