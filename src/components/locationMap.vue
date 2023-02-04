
/*
This is a test component to use vue2-leaflet package 
inside vue without coding JavaScript
but the package doesn't have some of needed options for the project 
and wasn't useful
*/

<template>
  <div class="locationMap">
    <l-map
      :zoom="6"
      :center="[47.313220, -1.319482]"
      style="height: 800px; width: 1000px"
    >
    <l-tile-layer 
      :url="url" 
      :attribution="attribution"
    />
    <l-geo-json
      :geojson="geojson"
      :options="options"
      :options-style="styleFunction"
    />
    <l-marker 
      :lat-lng="marker" 
      @click="toggleDetails()"
    >
      <l-tooltip >
        This is a tooltip
      </l-tooltip>
    </l-marker>
    </l-map>
    
    <div 
      v-show="details"
      class="container"  
    >
      <h1>
        City Name {{this.temprature}}
      </h1>
      <div>
        Lorem ipsum dolor sit amet consectetur, adipisicing elit. Corrupti consequatur
        ut obcaecati sed aut reiciendis, sint accusamus ad velit quia dolore in deleniti vero iste 
        voluptates animi vitae! Itaque, ex.
      </div>
    </div>
  </div>
</template>

<script>
// import geojson from '../components/alij.json'

export default {
  name: 'locationMap',
  props: {
    temprature: {
      type: Number,
      default: () => {
        return ''
      }
    }
  },
  data () {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: [47.313220, -1.319482],
      details: false,
      geojson: null,
      color: "#ECEFF1",
      fillColor: "red",
      loading: false,
      enableTooltip: true,
    };
  },


  methods: {
    toggleDetails(){
      this.details = !this.details;
    },
  },

  computed: {
    options() {
      return {
        onEachFeature: this.colorlayer
      };
    },
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 2,
          color: "#ECEFF1",
          fillColor: fillColor,
          fillOpacity: 1,
          // opacity:1
        }
      }
    },
  },

   async created() {
    this.loading = true;
    const response = await fetch("https://rawgit.com/gregoiredavid/france-geojson/master/regions/pays-de-la-loire/communes-pays-de-la-loire.geojson")
    const data = await response.json();
    this.geojson = data;
    this.loading = false;
    console.log(this.geojson)
  }
}



</script>

<style scoped>

.locationMap{
  display: flex;
}
.container{
  height: 100%;
  width: 40%;
  margin: 30px;
}
</style>
