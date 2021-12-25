<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <your-city></your-city>
  </div>
</template>

<script>
import yourCity from "../components/youCityComponent.vue";
export default {
  name: "Home",
  components: {
    yourCity,
  },
};
</script>
