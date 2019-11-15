<template>
  <div class="searchbar">
    <v-autocomplete :items="metroLines" rounded></v-autocomplete>
  </div>
</template>

<script>
import metroData from "@/data/seoulMetroData.json";

export default {
  data: () => ({
    metroLines: []
  }),
  methods: {
    initMetroLines() {
      for (let i = 0; i < metroData.length; i++) {
        /* eslint-disable no-console */
        this.metroLines.push(`${metroData[i].name}역 (${metroData[i].line})`);
      }
      this.metroLines.sort();
    }
  },
  mounted() {
    this.initMetroLines();
  }
};
</script>

<style scoped lang="scss">
.searchbar {
  width: 25%;
  display: flex;

  .v-input {
    padding-top: 20px;
    background-color: white;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  }
}
</style>