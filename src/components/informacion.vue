<template>
  <div>
    <footer>
      <p>Hecho por Wiliams Ixcoy, @wili_code</p>
    </footer>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    let github = ref("https://github.com/wiliCode");
    let instagram = ref("https://www.instagram.com/wili_code/");
    let twitter = ref("https://twitter.com/wiliCode");
    let linkedin = ref(
      "https://www.linkedin.com/in/wiliams-alexander-tzoc-ixcoy-656074229/"
    );

    return {
      github,
      instagram,
      linkedin,
      twitter,
    };
  },
};
</script>
