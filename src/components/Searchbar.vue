<template>
  <div class="searchbar">
    <v-autocomplete
      placeholder="지하철 역 검색"
      v-model="selectedStation"
      v-on:change="setStation(selectedStation)"
      :items="metroLines"
      rounded
      autofocus
    ></v-autocomplete>
  </div>
</template>

<script>
import metroData from "@/data/seoulMetroData.json";

export default {
  computed: {
    station() {
      return this.$store.state.station;
    }
  },
  data: () => ({
    metroLines: [],
    selectedStation: ""
  }),
  methods: {
    initMetroLines() {
      for (let i = 0; i < metroData.length; i++) {
        this.metroLines.push(`${metroData[i].name} (${metroData[i].line})`);
      }
      this.metroLines.sort();
    },
    setStation(selectedStation) {
      let info = selectedStation.split(" ");
      this.$store.state.station = info[0];
      this.$store.state.line = info[1].replace("(", "").replace(")", "");

      if (this.$store.state.station != null) {
        this.$store.state.locationSelected = true;
      } else {
        this.$store.state.locationSelected = false;
      }
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