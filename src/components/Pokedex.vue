<template>
  <div id="pokedex" v-if="Object.keys(pokemons).length>0">
  

  <div class="vl"></div>

  <div id="areaTela">

    <div id="tela">

      <div id="infosPokedex" v-for="pokemon in pokemons.slice(indice,indice+1)" :key="pokemon.name">
    
          {{show_pokemon(get_id(pokemon))}}
          <img v-if="image"
            :src="image" class="imgPokemon1">

          <div>
          <h2>{{get_name(pokemons[indice])}}</h2>
          <div v-if="type">Tipo: {{type}}</div>
          <br>
          <div v-if="weight">Peso: {{weight/2/2}}kg</div>
          <br>
          <div v-if="height">Altura: {{height/10}}m</div>
          </div>

      </div>
  
  </div>

    </div>
     <div id="botoes">
  <button @click="indice--" class="button"></button>
  <button @click="indice++" class="button"></button>
  </div>
</div>

</template>


<script>
import axios from 'axios';
import api from '../services/api';

export default {
  name: "Pokedex",
  components: {},
  data:() => {
    return {
      pokemons: [],
      selected_pokemon: null,
      weight: "",
      height: "",
      image: "",
      type:"",
      indice: Math.floor(Math.random() * 800) + 1
    }
  },
  created(){

    api.get(this.url).then((response) =>{
        // console.log(response)
        this.pokemons = response.data.results;
      })

  },

methods: {
  get_id(pokemon){
    return Number(pokemon.url.split("/")[6])
  },
  get_name(pokemon){
    return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
  },
  show_pokemon(id){
    axios
      .get(`https://pokeapi.co/api/v2/pokemon/${id}`)
      .then((response) =>{
        // console.log(response.data.weight)
        this.selected_pokemon = response.data
        this.image = response.data.sprites.front_default
        this.weight = response.data.weight
        this.height = response.data.height
        this.type = response.data.types[0].type.name
      })
  }

}


};
</script>




