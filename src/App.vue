<template>
  <div id="app">
    <Sidebar />
    <router-view />
  </div>
</template>
<script>
import Sidebar from "./components/Sidebar.vue"
export default {
  components: {
    Sidebar
  }
}
</script> 
<style lang="scss">
#app {
  display: flex;
}
body {
  font-family: Roboto, sans-serif;
  margin: 0;
  background: #E2E7ED;
}
</style>
