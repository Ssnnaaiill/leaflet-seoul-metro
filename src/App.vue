<template>
  <v-app>
    <v-content>
      <Map />
      <div class="top-searchbar">
        <Searchbar />
      </div>
    </v-content>
  </v-app>
</template>

<script>
import Map from "@/components/Map";
import Searchbar from "@/components/Searchbar";

export default {
  name: "App",
  components: {
    Map,
    Searchbar
  },
  data: () => ({
    //
  })
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

  .top-searchbar {
    width: 100%;
    display: flex;
    position: fixed;
    justify-content: center;
    z-index: 2;
    top: 10px;
  }
}
</style>
