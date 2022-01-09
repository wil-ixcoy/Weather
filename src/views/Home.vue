<template>
  <div class="home">
    <clima-actual></clima-actual>
    <p class="creador">Hecho por Wiliams Ixcoy</p>
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
