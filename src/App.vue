<template>
  <div id="app">
    
    <qr-scanner msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import QrScanner from './components/qrScanner.vue'

export default {
  name: 'App',
  components: {
    QrScanner
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  img {
      height: 20rem;
    }
</style>

