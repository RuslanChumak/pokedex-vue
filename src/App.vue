<template>
<v-app>
<v-container v-if='isLoaded' fluid>
    
    <v-pagination
      @input='changePage'
      v-model="page"
      :length="length"
      :page="page"
      total-visible="10"
      
    ></v-pagination>
    <v-row>
      <v-col cols='12' md='3'>
      <v-select
          max-width='200'
          @input='changeLimit'
          v-model="limit"
          :items="limitList"
          label="Limit"
      ></v-select>
    </v-col>
    <v-col cols='12' md='3'>
      <v-text-field label="Search" v-model="searchValue" @input='search' hide-details="auto"></v-text-field>
    </v-col>
    <v-col cols='12' md='3'>
      <v-select
          max-width='200'
          @input='changeType'
          v-model="type"
          :items="types"
          label="Filter by type"
    ></v-select>
    </v-col>
    </v-row>
    
    
    
    <v-row align="center" justify = 'space-around'> 
      <card :key='item.name' v-for="item in items" :url='item.url'></card>
    </v-row>
  </v-container>
<div v-if='!isLoaded'>Loading...</div>
</v-app>
  
</template>

<script>

import Card from './components/Card'
export default {
  
  
  components: {
    Card
  },
  mounted(){
    this.getTypes()
    fetch('https://pokeapi.co/api/v2/pokemon?limit=1000&offset=0').then(res => res.json())
    .then(res =>{
      this.allPokemons = res.results
      this.itemsForSearch = res.results
      this.isLoaded = true
      this.search(this.searchValue)
    })
  },
  data: () => ({
    allPokemons: [],
    items: [],
    filteredItems: [],
    itemsForSearch: [],
    isLoaded: false,
    page: 1,
    length:0,
    limit: 10,
    limitList: [10, 20, 50, 100],
    type: 'none',
    types: [],
    searchValue: ''
  }),
  methods:{
    changePage : function(num){
      this.page = num
      this.itemsOnPage()
    },
    changeLimit : function(num){
      this.limit = num
      this.page = 1
      this.itemsOnPage()
    },
    itemsOnPage: function(){
      this.items = this.filteredItems.slice((this.page - 1) * this.limit, this.page * this.limit)
    },
    getTypes: function(){
      fetch('https://pokeapi.co/api/v2/type').then(res => res.json())
      .then(res =>{
        this.types = res.results.map(item => item.name)
        this.types.unshift('none')
    })
    },
    changeType: function(type){
      this.type = type
      if(type == 'none'){
        this.itemsForSearch = this.allPokemons
        this.search(this.searchValue)
      }
      else{
        fetch(`https://pokeapi.co/api/v2/type/${type}`).then(res => res.json())
        .then(res =>{
          this.itemsForSearch = res.pokemon.map(item => item.pokemon)
          this.search(this.searchValue)
        })
      }
    },
    search: function(value){
      this.filteredItems = this.itemsForSearch.filter(item =>item.name.indexOf(value) === -1 ? false : true)
      this.length = Math.ceil(this.filteredItems.length / this.limit)
      this.changePage(1)   
    }
  }
};
</script>
<style scoped>
.row{
  justify-content: center;
}
</style>
