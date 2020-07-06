<template>
<div class='elofchain'>
    <img v-if='isLoaded' :src='data'>
    <v-progress-circular
      indeterminate
      v-if='!isLoaded'
    ></v-progress-circular>
</div>
    
</template>

<script>
export default {
    props: ["name"],
    mounted(){
        fetch("https://pokeapi.co/api/v2/pokemon/"+this.name).then(res => res.json()).then(r =>{
            this.data = r.sprites.front_default
            this.isLoaded = true
        })
    },
    data: () => ({
        data: '',
        isLoaded: false,
        
    })
}
</script>
<style scoped>
.elofchain{
    width:33%;
}
img{
    image-rendering: pixelated;
}
</style>

