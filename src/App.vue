<template>
  <div id="app">
    <div class="map">
      <locationMap 
        :temprature="temprature"
      />
    </div>
  </div>
</template>

<script>

import locationMap from './components/locationMap.vue'

export default {
  name: 'App',
  components: {
    locationMap,
  },
  data() {
    return {
      temprature : 50,  
    }
  },
}
</script>

<style>

</style>
