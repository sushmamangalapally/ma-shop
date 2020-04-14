<template>
  <div id="app">
    <ShoppingCart shopName="Market America"/>
  </div>
</template>

<script>
import "./assets/styleOutput.css"
import ShoppingCart from './components/ShoppingCart.vue'

export default {
  name: 'App',
  components: {
    ShoppingCart
  }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}

body {
    background-color: #f0f1f7;
    color: #333;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
