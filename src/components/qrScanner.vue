<template>
  <div class="hello">
    <div class="video">
      <video autoplay></video>
    </div>
   

    <canvas v-if="scanning" ref="canvas"></canvas>
    <img class="arrow" src="../assets/arrow-up.png" />
    <h3>Scan above</h3>
    <div ref="result" v-if="qrScanResult">
      <b>Data:</b> <span id="outputData">{{ result }}</span>
    </div>
  </div>
</template>

<script>
import QrScanner from 'qr-scanner'

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      scanning: false,
      qrcode: null,
      qrScanResult: false,
      result: "",
      image: true,
      videourl: "",
    };
  },
  methods: {
    tick() {
      const canvasElement = this.$refs.canvas.getContext('2d');
      const video = this.$refs.video;
      canvasElement.height = video.videoHeight;
      canvasElement.width = video.videoWidth;
      //canvas.drawImage(video, 0, 0, canvasElement.width, canvasElement.height);

      this.scanning = true && requestAnimationFrame(this.tick());
    },
    scan() {
      if (
        "mediaDevices" in navigator &&
        "getUserMedia" in navigator.mediaDevices
      ) {
        //let self = this;
        //this.scanning = true;
        // this.image = false;
        // const video = document.querySelector("video");
        // console.log(video);
        // this.qrScanResult = true;
        // let constraints = {
        //   video: {
        //     width: { min: 80 },
        //     height: { min: 50 },
        //     // facingMode: {
        //     //   exact: 'environment'
        //     // }
        //   },
        // };
        // navigator.mediaDevices.getUserMedia(constraints).then(function(stream) {
        //   console.log(stream);
           const video = document.querySelector('video');
        //   //self.scanning = true;
        //   //self.qrScanResult = true;
        //   //btnScanQR.hidden = true;
        //   //canvasElement.hidden = false;
        //   // const video = this.$refs.video;
        //   video.srcObject = stream;
        //   video.setAttribute("playsinline", true); // required to tell iOS safari we don't want fullscreen

        //   video.play();
         
          
        // });
         //this.tick();
         const qrScanner = new QrScanner(video, result => console.log('decoded qr code:', result));
          qrScanner.start();
          setTimeout(qrScanner.stop(), 4000)
      }
    },
    
    theScan() {
      try {
        this.qrcode.decode();
      } catch (e) {
        setTimeout(this.theScan(), 300);
      }
    },
  },
  watch: {
    qrcode: function() {
      this.qrcode.callback = (res) => {
        if (res) {
          const video = this.$refs.video;
          this.result = res;
          this.scanning = false;

          video.srcObject.getTracks().forEach((track) => {
            track.stop();
          });

          this.qrScanResult = false;
        }
      };
    },
  },
  beforeMount: function () {
		console.log(window.qrcode);
    this.qrcode = window.qrcode;
	},
  mounted() {
    this.scan();

    // const canvasElement = this.$refs.qr-canvas;
    // const canvas = canvasElement.getContext("2d");
    // console.log(canvasElement)
    //const outputData = this.$refs.outputData;
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 9px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.arrow {
  height: 5rem;
  display: block;
  margin: 0 auto;
}
img {
  height: 25rem;
}


@media only screen and (max-width: 600px) {
  .video {
    width: 80vw;
    margin: 0 auto;
  }
  video {
    max-width: 100%;
    max-height: 100%;
    overflow: hidden;
  }
}
</style>
