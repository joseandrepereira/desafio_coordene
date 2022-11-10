<template>
  <div id="app">
    <BestTransport />
  </div>
</template>

<script>
import BestTransport from './components/BestTransport.vue'

export default {
  name: 'App',
  components: {
    BestTransport
  }
}
</script>

<style scoped>
  #app{
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
</style>
