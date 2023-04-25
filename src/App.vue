<template>
  <div id="app">
    <calculator></calculator>
  </div>
</template>

<script>
import Calculator from "./components/Calculator.vue";
export default {
  components: {
    Calculator,
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  color: #2c3e50;
  margin-top: 50px;
}
</style>
