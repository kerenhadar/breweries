<template>
<div class="flex-col">
<div class="state-title">
  <h2>{{state.stateName}}</h2>
</div>
 <BreweryView  
 v-for="brewery in state.breweries" 
 :key="brewery.id" 
 :brewery="brewery" 
 :state="state.stateName" 
 v-on:deleteBrewery="deleteBrewery"
 v-on:saveBrewery="saveBrewery"/>
 </div>
</template>

<script>
import BreweryView from './BreweryView.vue'

export default {
  name: 'StateView',
  props: {
    state: null
  }  ,
  components: {    
    BreweryView
  },
  methods:{
    deleteBrewery(id){
      console.log(this.state.stateName);
      this.$emit("deleteBrewery", id, this.state.stateName);
    },
    saveBrewery(id, city, street){
      this.$emit("saveBrewery", this.state.stateName, id, city, street);
    },

  }
}
</script>
