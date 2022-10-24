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

  // mounted(){
  //   this.geojson = geojson;
  //   console.log(geojson.features)
  // },

  methods: {
    toggleDetails(){
      this.details = !this.details;
    },

    // getColor_by_temprature(temp){
    //   return temp < 9 ? '#800026' :   // <----- return the value
    //     temp < 50 ? '#BD0026' :
    //     temp < 3 ? '#E31A1C' :
    //     temp < 100 ? '#FC4E2A' :
    //             '#FFEDA0';
    // }
    
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
        };
      };
    },
    // colorlayer(feature, layer) {
    // layer.on('mouseover', function (e) {
    //     layer.setStyle({
    //         fillOpacity: 0.4
    //     });
    // });
    // layer.on('mouseout', function (e) {
    //     layer.setStyle({
    //         fillOpacity: 0
    //     });
    //   });
    // }

    //    if(this.temprature==50){
    //      return {fillColor:"red"}
    //    }
    //   }, 
      //  switch(this.temprature){
      //    case '30' : 
      //     fillColor: "red"
      //     return fillColor;
      //     break;

      //   case '50' :
      //     fillColor: "blue"
      //     return fillColor;
      //     break;

      //  }
     
  },

   async created() {
    this.loading = true;
    const response = await fetch("https://rawgit.com/gregoiredavid/france-geojson/master/regions/pays-de-la-loire/communes-pays-de-la-loire.geojson")
    const data = await response.json();
    this.geojson = data;
    this.loading = false;
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
