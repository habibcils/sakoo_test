<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <Cart msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Cart from './components/Cart.vue'

export default {
  name: 'App',
  components: {
    Cart
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
