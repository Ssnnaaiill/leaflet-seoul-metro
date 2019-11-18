<template>
  <v-app>
    <v-content>
      <router-view />
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "App"
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Nanum+Gothic:700,800&display=swap&subset=korean");

body {
  font-family: "Nanum Gothic", sans-serif !important;

  .v-content {
    width: 100vw;
    height: calc(100vh - 48px);
    display: flex;
  }
}
</style>
