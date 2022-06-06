<template>
  <Calcualtor />
</template>

<script>
import Calcualtor from "./components/Calcualtor.vue";

export default {
  components: { Calcualtor },
  name: "App",
};
</script>

<style>
*,
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: #5E6B7D;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
