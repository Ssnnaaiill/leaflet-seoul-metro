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
        let targetIndex = metroData.findIndex(
          data =>
            data.name === this.$store.state.station &&
            data.line === this.$store.state.line
        );

        if (targetIndex > -1) {
          let targetData = metroData[targetIndex];
          let targetCenter = new L.LatLng(
            Number(targetData.lon),
            Number(targetData.lat)
          );

          this.map.flyTo(targetCenter, 18);
          this.$store.state.locationSelected = false;
        }
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
      this.map = L.map("map").locate({
        setView: true,
        maxZoom: 18
      });
      this.tileLayer = L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        maxZoom: 18
      });
      this.tileLayer.addTo(this.map);
      this.map.setView(new L.LatLng(37.56, 126.988), 12);
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
    },
    initMarkers() {
      const targetData = metroData;

      for (let i in targetData) {
        let metroIcon = L.icon({
          iconUrl: require(`@/assets/${targetData[i].line}.png`),
          iconSize: [25, 25]
        });
        let marker = L.marker(
          [Number(targetData[i].lon), Number(targetData[i].lat)],
          { icon: metroIcon }
        );
        marker.bindTooltip(
          `<b>${targetData[i].name} (${targetData[i].line})</b><br/>
          ${targetData[i].address}`
        );
        this.markerGroup.addLayer(marker);
        marker.addTo(this.map);
      }
    }
  },
  mounted() {
    this.initMap();
    this.initLayers();
    this.initMarkers();
  }
};
</script>

<style scoped lang="scss">
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.css";
@import "../../node_modules/leaflet.markercluster/dist/MarkerCluster.Default.css";
@import "../../node_modules/leaflet/dist/leaflet.css";

#map {
  height: 100%;
  width: 100%;
  z-index: 1;
}
</style>
