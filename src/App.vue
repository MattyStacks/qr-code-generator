<template>
  <div>
    <QRCodeGenerator />
  </div>
  <!-- <div>
    <NewQRCodeGenerator />
  </div> -->
  <div class="footer">
    <SiteFooter />
  </div>
</template>

<script>
import SiteFooter from './components/SiteFooter.vue';
import QRCodeGenerator from '@/components/QRCodeGenerator.vue'
// import NewQRCodeGenerator from '@/components/NewQRCodeGenerator.vue'

export default {
  name: 'App',
  components: {
    QRCodeGenerator,
    SiteFooter
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
.container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.footer {
  margin-top: 400px;
  /* Margin bottom by footer height */
  /* margin-bottom: 60px; */
  /* background-color: green; */
  /* color: white; */
  padding: 10px;
}
</style>
