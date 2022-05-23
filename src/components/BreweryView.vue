<template>
  <div class="brewery-card">
  <strong> {{ brewery.id }}</strong>
  <div class="flex-row">
    <div class="flex-col brewery-data">
      <div class="flex-row">
        <div class="m-1 label">
          city:
        </div>   
        <div v-if="isEdited" class="m-1"> 
          <input type="text" v-model="city" />
        </div>
        <div v-else class="m-1"> 
          {{ brewery.city }}
        </div>
      </div>   
      
      <div class="flex-row">
        <div class="m-1 label">
          street:
        </div>   
        <div v-if="isEdited" class="m-1"> 
          <input type="text" v-model="street" />
        </div>
        <div v-else class="m-1"> 
          {{ brewery.street }}
        </div>
      </div>
    </div>
    <div class="buttons flex-row">  
      
      <div v-if="isEdited">
        <button @click="saveBrewery()">Save</button>
      </div>
      <div v-else>
        <button @click="editBrewery()">Edit</button>
        <button @click="deleteBrewery()">Delete</button>
      </div>
    
    </div>
    </div>  
  </div>
</template>

<script>
export default {
  name: 'BreweryView',
  props: {
    brewery: null    
  },
  data(){
    return {
      isEdited : false,
      city: null,
      street: null      
    }
  },
  mounted: function(){
    this.city=this.brewery.city;
    this.street=this.brewery.street
  },
  methods:{
    deleteBrewery(){
      this.$emit("deleteBrewery", this.brewery.id)
    },
    editBrewery(){
      this.isEdited = true;
    },
    saveBrewery(){
      if (!this.city){
         alert("city is mandatory");         
         return;
       }
      this.$emit("saveBrewery", this.brewery.id, this.city, this.street);
      this.isEdited = false;
    }
  }

}
  </script>
