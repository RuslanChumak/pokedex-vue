<template>
<div>
    <v-card v-if='isLoaded' flat>
        <v-card-text>{{ data[0].effect }}</v-card-text>
    </v-card>
    <v-progress-circular
      indeterminate
      v-if='!isLoaded'
    ></v-progress-circular>
</div>
    
</template>

<script>
export default {
    props: ["url"],
    mounted(){
         fetch(this.url).then(res => res.json())
        .then(res =>{
            this.data = this.ability = res.effect_entries.filter(item => item.language.name == 'en')
            this.isLoaded = true
        })
    },
    data: () => ({
        data: {},
        isLoaded: false,
        
    })
   
    

}
</script>
<style scoped>
 
  
</style>
