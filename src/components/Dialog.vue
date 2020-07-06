<template>
    <v-dialog max-width='600'  v-model='isOpen' > 
        <v-card>
            <v-card-title class="headline">
                {{data.name[0].toUpperCase() + data.name.slice(1)}}
            </v-card-title>
            <img :src='data.sprites.front_default' >
            <v-simple-table>
                <template v-slot:default>
                    <thead>
                    
                    </thead>
                    <tbody>
                    <tr>
                        <td>Base Experience</td>
                        <td>{{ data.base_experience }}</td>
                    </tr>
                    <tr>
                        <td>Height</td>
                        <td>{{ data.height }}</td>
                    </tr>
                    <tr>
                        <td>Weight</td>
                        <td>{{ data.weight }}</td>
                    </tr>
                    <tr>
                        <td>Types</td>
                        <td><span :key='type.type.name' v-for='type in data.types' :class='type.type.name'>{{type.type.name}}</span></td>
                    </tr>
                    <tr :key='stat.stat.name' v-for='stat in data.stats'>
                        <td>{{stat.stat.name.toUpperCase()}}</td>
                        <td>{{stat.base_stat}}</td>
                    </tr>
                    </tbody>
                </template>
            </v-simple-table>
            <div>
                <v-tabs
                v-model="tab"
                background-color="primary"
                dark
                >
                <v-tab
                    v-for="ability in data.abilities"
                    :key="ability.ability.url"
                >
                    {{ ability.ability.name }}
                </v-tab>
                </v-tabs>

                <v-tabs-items v-model="tab">
                <v-tab-item
                    v-for="ability in data.abilities"
                    :key="ability.ability.name"
                >
                    <Ability :url='ability.ability.url'></Ability>
                </v-tab-item>
                </v-tabs-items>
            </div>
            <EvoChain :url='data.species.url'></EvoChain>
            <v-card-actions>
            <v-spacer></v-spacer>
                <v-btn class="ma-2" outlined color="indigo" v-on:click='close'>Close</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
import Ability from './Ability'
import EvoChain from './EvoChain'
export default {
    props: ['isOpen', 'data','close'],
    data: () =>({
        tab: null
    }),
    components:{
        Ability,
        EvoChain
    }
}
</script>
<style scoped>
    img{
        width:300px;
        image-rendering: pixelated;
        margin: 0 140px;
    }
    .container{
        background: white;
        padding:20px;
    }
</style>