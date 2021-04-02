<template>
  <lyte-app></lyte-app>
</template>

<script>
import LyteApp from "./components/LyteApp.vue";
export default {
  name: "App",
  components: { LyteApp },
};
</script>

<style lang="scss">
* {
  font-family: "Poppins";
}
body {
  margin: 0 !important;
  padding: 0 !important;
  border: 0 !important;
  color: white;
}
</style>