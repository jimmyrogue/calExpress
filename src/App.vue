<!--
 * @Description: 
 * @version: 
 * @Author: jimmy
 * @Date: 2022-02-11 17:47:21
 * @LastEditors: jimmy
 * @LastEditTime: 2022-02-11 19:14:18
-->
<template>
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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
