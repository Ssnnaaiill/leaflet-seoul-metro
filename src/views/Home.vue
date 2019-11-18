<template>
  <div class="wrapper">
    <Map />
    <div class="top-searchbar">
      <Searchbar />
    </div>
  </div>
</template>

<script>
import Map from "@/components/Map";
import Searchbar from "@/components/Searchbar";

export default {
  components: {
    Map,
    Searchbar
  }
};
</script>

<style lang="scss">
.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;

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
