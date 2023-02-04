<template>
  <div class="container">
    <div id="mapContainer"></div>
  </div>
</template>

<script>
//////////-----importing neccessary libraries & files-----//////////
import "leaflet/dist/leaflet.css";
import L from "leaflet";
import geojson from "../../Province1.json";
import mockData from "../../test.json";

export default {
  name: "leafletMap",
  // props:{
  //   type: Boolean,
  //   default: () =>{
  //     return false
  //   }
  // },

  data() {
    return {
      center: [32.87255939010237, 53.781741816799745],
      showDetailCard: false,
      // markerLatLng: [47.313220, -1.319482],
    };
  },

  methods: {
    /////////-----sets up leaflet map & dependancies on mount-----//////////
    setupLeafletMap: function () {
      //////////-----setting up map layer-----//////////
      const mapDiv = L.map("mapContainer").setView(this.center, 5);
      L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
        attribution:
          '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        // maxZoom: 18,
      }).addTo(mapDiv);

      //////////-----styling geoJson layer-----//////////
      var myStyle = {
        // "fillColor": "#ADD8E6",
        color: "black",
        weight: 2,
        opacity: 0.65,
        fillOpacity: 0,
        dashArray: 3,
        // "clickable": true
      };

      //////////-----declaring icon class-----//////////
      var phenomenonIcon = L.Icon.extend({
        options: {
          iconSize: [38, 35],
          // shadowUrl: require("../assets/images/marker-shadow.png"),
          // iconAnchor:   [22, 94],
          // popupAnchor: [-3, -76],
        },
      });

      /////////-------declaring each icon-----//////////
      var rainIcon = new phenomenonIcon({
          iconUrl: require("../assets/images/rain-orange.png"),
        }),
        snowIcon = new phenomenonIcon({
          iconUrl: require("../assets/images/snow-orange.png"),
        }),
        windIcon = new phenomenonIcon({
          iconUrl: require("../assets/images/wind-red.png"),
        }),
        cloudyIcon = new phenomenonIcon({
          iconUrl: require("../assets/images/partly-cloudy-yellow.png"),
        });

      /////////-------showing Icons according to zoom level-----//////////
        var IconsLayer = new L.FeatureGroup();

        mapDiv.on('zoomend', function() {
          if (mapDiv.getZoom() <6){
            mapDiv.removeLayer(IconsLayer);
          }
          else {
            mapDiv.addLayer(IconsLayer);
            }
        });

      /////////-----setting up geoJson-----//////////
      L.geoJSON(geojson, {
        style: myStyle,
        onEachFeature: (feature, layer) => {

          ///////////-----removes icon layers when map is first loaded--------//////
          mapDiv.removeLayer(IconsLayer)

          //////////-----sets color of each polygon according to api-----//////////
          const countryId = feature.id;
          for (let apiId in mockData) {
            if (apiId == countryId) {
              layer.setStyle({
                fillColor: getRegionColor(mockData[apiId].countryColor),
                fillOpacity: 0.7,
              });

              /////////-----zooms on colored country on click-----//////////
              layer.on("click",() =>{
                mapDiv.flyToBounds(layer.getBounds());
                this.$emit("provinceDetail");
              });

              ///////////-----sets icons of polygons according to api--------//////
              var polyCenter = layer.getBounds().getCenter();
              var phenomenon = L.marker(polyCenter, { icon: getRegonIcon(mockData[apiId].iconId) })
              .on('click',()=> this.toggleDetail())

              IconsLayer.addLayer(phenomenon)

              .addTo(mapDiv);
            }
          }

          /////////-----sets style on hover-----//////////
          layer.on("mouseover", function () {
            layer.setStyle({
              weight: 4,
              opacity: 1,
              dashArray: 1,
            });
          });

          //////////-----sets default style on mouseout-----//////////
          layer.on("mouseout", function () {
            this.setStyle({
              weight: 2,
              opacity: 0.65,
              dashArray: 3,
            });
          });

          ///////////////////////////--------------CLICK(need to fix)-------------///////////////////////
          // layer.on('click', function () {
          //   layer.setStyle({
          //     weight: 4,
          //     opacity: 1,
          //     dashArray: 1,
          //   });
          // })

          // layer.bindPopup(countryId.toString());       //popUp to see feature Id for debugging
        },
      }).addTo(mapDiv);

      //////////-----returns color according to color id in api-----//////////
      function getRegionColor(colorId) {
        switch (colorId) {
          case "1":
            return "red";
          // break;
          case "2":
            return "orange";
          // break;
          case "3":
            return "yellow";
          // break;
          default:
            return "blue"; /////////////////////////////////////come back////////////////////
        }
      }

      function getRegonIcon(iconId) {
        switch (iconId) {
          case '1':
            return rainIcon;
          // break;
          case '2':
            return snowIcon;
          // break;
          case '3':
            return windIcon;
          // break;
          case '4':
            return cloudyIcon;
          // break;
          default:
            return "blue";
        }
      }
    },

    toggleDetail() {
      this.$emit("NameEmit");
      // console.log(mockData[this.apiId].iconId)
    },

  },

  mounted() {
    this.setupLeafletMap();
  },
};
</script>

<style scoped>
#mapContainer {
  height: 580px;
}
</style>
