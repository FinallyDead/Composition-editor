<template>
  <div id="app">
       <ServiceCompositionEditor />
  </div>
</template>

<script>

import ServiceCompositionEditor from "@/components/ServiceCompositionEditor"
export default {
  name: 'App',
  components: {
     ServiceCompositionEditor
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
