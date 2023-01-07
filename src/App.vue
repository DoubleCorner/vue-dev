<template>
  <div id="app">
    <geojson />
  </div>
</template>

<script>
import Geojson from "@/components/Geojson.vue";

export default {
  name: "App",
  components: {
    Geojson,
  },
};
</script>

<style lang="less">
html {
  height: 100%;
  margin: 0;
  body {
    height: 100%;
    margin: 0;
    #app {
      height: 100%;
    }
  }
}
</style>
