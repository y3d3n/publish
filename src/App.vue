<template>
    <PublishHome msg="Welcome to Your Vue.js App"/>
</template>

<script>
import PublishHome from './components/PublishHome.vue'

export default {
  name: 'App',
  components: {
    PublishHome
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
body{
  margin: 0;
}
</style>
