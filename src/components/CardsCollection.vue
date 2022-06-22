<template>

  <div id="folder" v-if="Object.keys(pokemons).length>0">

    <div id="inputHeader">
     <input
          v-model="search"
          label="Pesquisar"
          placeholder="Digite o nome do pokemon"
        >
      </div>

<div id="cardsCollection">
 <div id="cards" v-for="pokemon in filtered_pokemons" :key="pokemon.name">
    

  <div id="framePokemon">
 <img 
    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
      pokemon)}.png`"
      :alt="pokemon.name"
      >


</div>
      <span>{{ get_name(pokemon) }}</span>

  </div>
  </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: "Pokedex",
  components: {},
  data:() => {
    return {
      pokemons: [],
      search: ""
    }
  },
  mounted(){
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=1000")
      .then((response) =>{
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
  }
},

  computed: {

    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  }


};
</script>

