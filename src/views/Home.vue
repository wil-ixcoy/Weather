<template>
  <div class="home">
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
<style lang="scss">
@import "../sass/main.scss";
</style>
