<template>
  <div class="home">
    <clima-actual></clima-actual>
    <p>Hecho por Wiliams Ixcoy, @wili_code</p>
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
