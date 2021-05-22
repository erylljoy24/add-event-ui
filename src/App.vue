<template>
  <div id="app">
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark fixed-top" style="height:40px; background-color: #787878; padding-bottom: 3px; padding-top: 3px;">
      
    </nav>
    <HelloWorld style="margin-top:50px"/>
  </div>
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

</style>
