<template>
  <div class="form-title">
    Add Brewery
  </div>
  
  <div class="brewery-card">
  <div class="flex-row">
    <div class="flex-col brewery-data">
      <div class="flex-row">
        <div class="m-1 label">
          *State
        </div>   
        <div class="m-1"> 
          <input type="text" v-model="state" />
        </div>        
      </div>   
      <div class="flex-row">
        <div class="m-1 label">
          *Brewery id
        </div>   
        <div class="m-1"> 
          <input type="text" v-model="breweryId" />
        </div>
      </div>        
      <div class="flex-row">
        <div class="m-1 label">
          *City
        </div>   
        <div class="m-1"> 
          <input type="text" v-model="city" />
        </div>        
      </div>
      <div class="flex-row">
        <div class="m-1 label">
          Street
        </div>   
        <div class="m-1"> 
          <input type="text" v-model="street" />
        </div>        
      </div>
    </div>
    <div class="buttons flex-row">
        <button @click="addBrewery()">Add</button>
        <button @click="closeAddBrewery()">Cancel</button>
    </div>
    </div>  
  </div>
</template>

<script>
export default {
  name: 'AddBreweryView',
  data(){
    return {
      state: null,
      breweryId: null,
      city: null,
      street: null      
    }
  },  
  methods:{    
    addBrewery(){
      if(!(this.state && this.breweryId && this.city)){
        alert("missing data");
        return;
      }
      this.$emit("addBrewery", this.state, this.breweryId, this.city, this.street);  
    },
    closeAddBrewery(){
      this.$emit("closeAddBrewery");
    }
  }

}
  </script>
