<template>
  <div id="app">
    <mainPage :persons="persons"></mainPage>
  </div>
</template>

<script>
import persons from "../data.json"
import mainPage from "@/components/mainPage";

export default {
  name: 'App',
  components: {
    mainPage
  },
  data() {
    return {
      persons,
    }
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
  display: flex;
  justify-content: center;
}
</style>
