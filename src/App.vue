<template>
  <div id="app">
    <img width="auto" src="./assets/Vue-logo.png">
    <DropboxSelector/>
  </div>
</template>

<script>
import DropboxSelector from "./components/DropboxSelector";

export default {
  name: "App",
  components: {
    DropboxSelector
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
