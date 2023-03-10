<template>
  <div>
    <QRCodeGenerator />
  </div>
  <!-- <div>
    <NewQRCodeGenerator />
  </div> -->
</template>

<script>
import QRCodeGenerator from '@/components/QRCodeGenerator.vue'
// import NewQRCodeGenerator from '@/components/NewQRCodeGenerator.vue'

export default {
  name: 'App',
  components: {
    QRCodeGenerator
    // NewQRCodeGenerator
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
</style>
