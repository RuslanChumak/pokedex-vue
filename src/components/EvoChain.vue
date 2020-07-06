<template>
<div>
    <div class='flex' v-if='isLoaded'>
        <ElOfChain :key='el' v-for='el in data' :name='el'></ElOfChain>
    </div>
    <v-progress-circular
      indeterminate
      v-if='!isLoaded'
    ></v-progress-circular>
</div>
    
</template>

<script>
import ElOfChain from './ElOfChain'
export default {
    props: ["url"],
    mounted(){
         fetch(this.url).then(res => res.json())
        .then(res =>{
            fetch(res.evolution_chain.url).then(res => res.json()).then(r =>{
                let el = r.chain
                do{
                    this.data.push(el.species.name)
                    el = el.evolves_to[0]
                }while(el !== undefined)
                this.isLoaded = true
                })
        })
    },
    data: () => ({
        data: [],
        isLoaded: false,
        
    }),
    components:{
        ElOfChain
    }
   
    

}
</script>
<style scoped>
 .flex{
     display:flex;
 }
</style>
