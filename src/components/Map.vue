<template>
  <div id="map"></div>
</template>

<script>
import L from "leaflet";
import metroData from "@/data/seoulMetroData.json";
import "../../node_modules/leaflet.markercluster/dist/leaflet.markercluster.js";

export default {
  computed: {
    locationSelected() {
      return this.$store.state.locationSelected;
    }
  },
  watch: {
    locationSelected: function(selected) {
      if (selected) {
        let provinceStateStation = this.$store.state.station;
        let provinceStateLine = this.$store.state.station;

        const targetDataIndex = metroData.findIndex(data => {
          data.name === provinceStateStation && data.line === provinceStateLine;
        });
      }

      if (targetDataIndex > -1) {
        this.clearMap();
      }
    }
  },
  data: () => ({
    map: null,
    tileLayer: null,
    markerGroup: null
  }),

  methods: {
    initMap() {
      this.map = L.map("map", { doubleClickZoom: true }).locate({
        setView: true,
        maxZoom: 16
      });
      this.tileLayer = L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        maxZoom: 18
      });
      this.tileLayer.addTo(this.map);
      this.map.setView(new L.LatLng(37.56, 126.985), 12);

      let targetData = metroData;

      for (let i = 0; i < targetData.length; i++) {
        let metroIcon = L.icon({
          iconUrl: require(`@/assets/${targetData[i].line}.png`),
          iconSize: [20, 20]
        });
        let marker = L.marker(
          [Number(targetData[i].lon), Number(targetData[i].lat)],
          { icon: metroIcon }
        );
        marker.bindTooltip(
          `${metroData[i].name}역 (${metroData[i].line})<br/>
          ${metroData[i].address}`
        );
        marker.addTo(this.map);
      }
    },
    initLayers() {
      this.markerGroup = L.markerClusterGroup({
        chunkedLoading: true,
        spiderfyOnMaxZoom: false,
        zoomToBoundsOnClick: false
      });
      this.markerGroup.on("clusterclick", cluster =>
        this.onClickCluster(cluster)
      );
      this.map.addLayer(this.markerGroup);
    }
  },
  clearMap() {
    for (let i in this.map._layers) {
      if (this.map._layers[i]._path != undefined) {
        this.map.removeLayer(this.map._layers[i]);
      }
    }
  },
  mounted() {
    this.initMap();
    this.initLayers();
  }
};
</script>

<style>
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.css";
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.Default.css";
@import "../../node_modules/leaflet/dist/leaflet.css";

#map {
  height: 100%;
  width: 100%;
  z-index: 1;
}
</style>
