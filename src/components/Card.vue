<template>
<div>
    <v-card v-if='isLoaded' v-on:click='toggleDialog'>
        <Dialog :isOpen='isOpen' :data='data' :close='toggleDialog'></Dialog>
        <v-card-title>{{data.name[0].toUpperCase() + data.name.slice(1)}}</v-card-title>
        <v-card-subtitle>
            <span :key='type.type.name' v-for="type in data.types" :class="type.type.name">{{type.type.name}}</span>
        </v-card-subtitle>
        <img
            :src="data.sprites.front_default"
          >
    </v-card>
    <v-progress-circular
      indeterminate
      v-if='!isLoaded'
    ></v-progress-circular>
</div>
    
</template>

<script>
import Dialog from "./Dialog"
export default {
    props: ["url"],
    mounted(){
         fetch(this.url).then(res => res.json())
        .then(res =>{
            this.data = res
            this.isLoaded = true
        })
    },
    data: () => ({
        data: {},
        isLoaded: false,
        isOpen: false
    }),
    methods:{
        toggleDialog: function(){
            this.isOpen = !this.isOpen;
        }
    },
    components: {
        Dialog
    }

}
</script>
<style scoped>
  img{
    image-rendering: pixelated;
    width:200px;
    margin:10px;
  }
  .v-card{
    margin: 20px;
    color: black;
  }
  .v-card__title{
      text-align: center;
      font-size: 20px;
      font-weight: bold;
  }
  
</style>
