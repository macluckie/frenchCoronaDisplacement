<template>
  <div id="map"></div>
</template>

<script>
export default {
  name: "Map",
  data() {
    return {
      message: ""
    };
  },
  methods: {
    getCoordonnees: function(lat, lon) {
      var macarte = null;
      macarte = L.map("map").setView([lat, lon], 14);
      L.tileLayer("https://{s}.tile.openstreetmap.fr/osmfr/{z}/{x}/{y}.png", {
        attribution:
          'données © <a href="//osm.org/copyright">OpenStreetMap</a>/ODbL - rendu <a href="//openstreetmap.fr">OSM France</a>',
        minZoom: 1,
        maxZoom: 20
      }).addTo(macarte);

      L.marker([lat, lon])
        .addTo(macarte)
        .bindPopup("Vous ètes ICI")
        .openPopup();

      L.circle([lat, lon], { radius: 100000 }).addTo(macarte);
    }
  },

  async mounted() {
    if ("geolocation" in navigator) {
      navigator.geolocation.getCurrentPosition(
        position => {
          console.log(position.coords.latitude);
          console.log(position.coords.longitude);
          this.getCoordonnees(
            position.coords.latitude,
            position.coords.longitude
          );
        },
        error => {
          console.log(error);
          this.getCoordonnees(process.env.VUE_APP_LAT, process.env.VUE_APP_LON);
        },
        { maximumAge: 75000 }
      );
    } else {
    }
  }
};
</script>

<style scoped>
#map {
  height: 800px;
}
</style>