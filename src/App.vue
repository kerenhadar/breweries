<template>
  <div  :class="{ 'dark-bg': isAdd}"> 
  <div class="title">
    Breweries
  </div>
  <div class="top">
    <button @click="openAddBrewery()">Add Brewery</button>
  </div>
    
    <StateView  
    v-for="state in stateBreweries" 
    :key="state.stateName" 
    :state="state"
    v-on:deleteBrewery="deleteBrewery"
    v-on:saveBrewery="saveBrewery"
    />

    <div v-if="isAdd" class="modal">
    
      <div class="modal-content">
        <AddBrewery 
        v-on:closeAddBrewery="closeAddBrewery" 
        v-on:addBrewery="addBrewery" />
      </div>
    </div>
  </div>
</template>

<script>
import StateView from './components/StateView.vue';
import AddBrewery from './components/AddBrewery.vue';
import axios from "axios";

class BreweryData {
    constructor(id, data) {
        this.id = id;
        this.city = data.city;
        this.street = data.street;
    }
}

class StateData {
  breweries = [];
  
  constructor(data) {
      this.stateName = data.stateName;
      this.breweries = Object.keys(data.breweries).sort(x => x.city).map(key => {
          return new BreweryData(key, data.breweries[key]);
      });
      this.sortByCity();
  }

  sortByCity(){
    this.breweries.sort((a,b)=>{ return a.city.toLowerCase() > b.city.toLowerCase() ? 1 : -1 })
  }    
}


export default {
  name: 'App',
  components: {    
    StateView,
    AddBrewery
  },
  data(){
    return {
            name: "Keren",
            stateBreweries: [],
            isAdd: false,
            mainObj:{}
        };
  },
  mounted: function (){    
    axios
    .get("https://api.openbrewerydb.org/breweries")
    .then(res =>{
      let breweriesRawData = res.data;
            let states = {};

            breweriesRawData.forEach(item => {
                states[item.state] = states[item.state] ?? { "stateName": item.state, "breweries": {} };
                states[item.state].breweries[item.id] = { "city": item.city, "street": item.street };
            });

            this.stateBreweries = Object.keys(states).sort().map(key => {
                return new StateData(states[key]);
            });

            this.mainObj = {states: states}
    });
    
  },
  methods:{
    deleteBrewery(breweryId,stateName){
      let breweries =
          this.stateBreweries
              .find(x => x.stateName == stateName)
              .breweries
              .filter(x => x.id != breweryId);

      this.stateBreweries
          .find(x => x.stateName == stateName)
          .breweries = breweries;

      delete this.mainObj.states[stateName].breweries[breweryId];

      if(Object.keys(this.mainObj.states[stateName].breweries).length == 0){
          delete this.mainObj.states[stateName];
          this.stateBreweries = this.stateBreweries.filter(x=>x.stateName!==stateName);
      }
      console.log(this.mainObj);      
    },
    saveBrewery(stateName, breweryId, city, street){
      if (!(stateName && breweryId && city)){
        alert("missing data");         
        return;
      }
      let brewery =
          this.stateBreweries
              .find(x => x.stateName == stateName)
              .breweries
              .find(x => x.id == breweryId);
      if (brewery){
        brewery.city = city;
        brewery.street = street;         
      } 

      this.stateBreweries.find(x=> x.stateName==stateName ).sortByCity();

      this.mainObj.states[stateName].breweries[breweryId]={city: city, street: street};  
      console.log(this.mainObj);             
    },
    addBrewery(stateName, breweryId, city, street){
      let brewery = new BreweryData(breweryId, {state: stateName, city: city, street: street});

      let state = this.stateBreweries.find(x=>{if(x.stateName == stateName) return true;}); 
    
      if(state){
        state.breweries = [...state.breweries, brewery].sort((a,b)=>{return a.city>b.city? 1 : -1}); 
        
        state.sortByCity();
        
        this.mainObj.states[stateName].breweries[breweryId] = {city: city, street: street};

      }else {        
        let stateObj =  {stateName: stateName, breweries: {[breweryId]:{city: city, street: street}}};  

        this.stateBreweries = [...this.stateBreweries, new StateData(stateObj)];        
        
        this.mainObj.states = {...this.mainObj.states, [stateName]: {...stateObj}};
      }

      this.stateBreweries.sort((a,b) => {return a.stateName.toLowerCase() > b.stateName.toLowerCase() ? 1 : -1});

      console.log(this.mainObj); 
      this.isAdd=false;           
    },
    openAddBrewery(){
      this.isAdd = true;
    },
    closeAddBrewery(){
      this.isAdd = false;
    }
}}
</script>

<style>
@import './assets/style.css';

</style>
