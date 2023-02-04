<template>
  <div id="app">
    <div class="wrapper">
      <div class="map">
        <leafletMap 
          v-on:NameEmit="showIconCard()"
          v-on:provinceDetail="showProvinceCard()"
        />
      </div>
  
      <div
        v-if="showIconDetails"
        class="test"
      >
      <transition name="fade">
        <detailscard 
          :msg="text"
          class="card"
        />
      </transition>
      </div>

      <div
        v-if="showProvinceDetails && !showIconDetails"
        class="test"
      >
        <provinceDetailsCard />
      </div>

    </div>
  </div>
</template>

<script>

// import locationMap from './components/locationMap.vue'
import leafletMap from './components/leafletMap.vue'
import detailscard from './components/iconDetailsCard.vue'
import provinceDetailsCard from './components/provinceDetailsCard.vue'

export default {
  name: 'App',
  components: {
    leafletMap, 
    detailscard,
    provinceDetailsCard
  },

  data() {
    return {
      showIconDetails: false,
      showProvinceDetails: false,
      text: "This is Icon details :)"
    }
  },
  methods:{
    /**when you click on province , then icon and then AGAIN on province 
     * both cards appear at the same time for a moment */
    showIconCard(){
      this.showProvinceDetails = false
      this.showIconDetails = !this.showIconDetails
    },

    showProvinceCard(){
      this.showIconDetails = false
      this.showProvinceDetails = !this.showProvinceDetails
    }
  }
}
</script>

<style>

body{
  margin: 0;
  padding: 0;
}

.map{
  width: 70%;
}

/* .card{
  width: 20%;
  margin: 10px;
} */
.wrapper{
  display:flex;

}

.test{
  padding: 10px;
  transition: all 0.8s ease;
  max-height: 100vh;
  max-width: 28%;
}

.card{
  transition: all 0.8s ease;  /*not working because of v-if(triggers display property) */
}

</style>
