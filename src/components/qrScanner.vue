<template>
  <div class="hello">
    <a ref="qr-btn" @click="scan" >
      <img alt="Vue logo" src="../assets/qrcode.jpg">
     
      <video v-if="scanning" ref="video" :src="videourl"  id="thevideo">
      </video>
    </a>

    <canvas v-if="scanning" ref="qr-canvas"></canvas>
     <img class="arrow" src="../assets/arrow-up.png" />
    <h3>Click the bar-code to scan</h3>
    <div ref="result" v-if="qrScanResult">
      <b>Data:</b> <span id="outputData">{{ result }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      scanning : false,
      qrcode: null,
      qrScanResult: false,
      result: "",
      image: true,
      videourl : ""
    }
  },
  methods: {
    scan() {
      let self = this
      //this.scanning = true;
      this.image= false;
      const video = document.getElementById('#thevideo');
      console.log(video)
      this.qrScanResult = true;
      navigator.mediaDevices
      .getUserMedia({ video: { facingMode: "environment" } })
      .then(function(stream) {
         self.scanning = true;
        self.qrScanResult = true;
        //btnScanQR.hidden = true;
        //canvasElement.hidden = false;
        // const video = this.$refs.video;
        self.videourl = stream;
        video.setAttribute("playsinline", true); // required to tell iOS safari we don't want fullscreen
        
        video.play();
        this.tick();
        this.theScan();
      });
    },
    tick() {
      const canvasElement = this.$refs.qr-canvas;
      const canvas = canvasElement.getContext("2d");
      const video = this.$refs.video;
      canvasElement.height = video.videoHeight;
      canvasElement.width = video.videoWidth;
      canvas.drawImage(video, 0, 0, canvasElement.width, canvasElement.height);

      this.scanning = true && requestAnimationFrame(this.tick());
    },
    theScan() {
      try {
        this.qrcode.decode();
      } catch (e) {
        setTimeout(this.theScan(), 300);
      }
    }
  },
  watch: {
    qrcode: function() {
      this.qrcode.callback = (res) => {
        if (res) {
          const video = this.$refs.video;
          this.result = res;
          this.scanning = false;

          video.srcObject.getTracks().forEach(track => {
            track.stop();
          });

          this.qrScanResult = false;
        }
      };
    }
  },
  mounted() {
    console.log(window.qrcode)
    this.qrcode = window.qrcode


    // const canvasElement = this.$refs.qr-canvas;
    // const canvas = canvasElement.getContext("2d");
    // console.log(canvasElement)
    //const outputData = this.$refs.outputData;

  }
}
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
</style>
