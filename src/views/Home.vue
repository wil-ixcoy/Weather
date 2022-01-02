<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <clima-actual></clima-actual>
  </div>
</template>

<script>
import climaActual from "../components/climaActual.vue";
export default {
  name: "Home",
  components: {
    climaActual,
  },
};
</script>
