<template>
  <div id="app">
    <v-app>
      <div class="wrapper">
        <img alt="Vue logo" src="./assets/logo.png" >
        <MetaMask/>
      </div>
    </v-app>
  </div>
</template>

<script>
import MetaMask from './components/MetaMask.vue'

export default {
  name: 'app',
  components: {
    MetaMask
  }
}
</script>

<style>
.wrapper{
  max-width: 500px;
  margin: 0 auto;
}
.wrap-word{
  text-align: center;
  word-break: break-all;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
