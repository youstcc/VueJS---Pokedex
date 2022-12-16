<template>
  <div class="list" v-if = "Object.keys( pokemons ).length > 0">
    <article @click="setPokemon( pokemon )" v-for="pokemon in pokemonsFiltered" :key = pokemon.name>
        <img :src = "imgurl + pokemon.name + '.png'">
        <h3>{{ pokemon.name }}</h3>
    </article>
  </div>
</template>

<script>
import axios from "axios";
import json from "../config/config.json";

export default {
  name: "list",
  props: [ "searchedPoke" ],
  data() {
    return {
      pokemons: {},
      imgurl: json.IMG_URL
    }
  },
  methods: {
    setPokemon( pokemon ) {
      // On ajoute les pokemons séléctionés dans le PokemonList
      this.$emit( "showpokemon", pokemon );
    }
  },
  computed: {
    // Variable de filtration des pokemons
    pokemonsFiltered() {
      var search = this.searchedPoke;
      // Si le bar de recherche est vide, on retourne tous les pokmeons
      if ( search.length == 0 ) return this.pokemons;
      // Else, returns the pokemons who have the search bar's content in their name
      return this.pokemons.filter( pokemon => pokemon.name.includes( search.toLowerCase() ) );
    }
  },
  async created() {
    // on appelle l'API pour obtenir les pokemons
    const response = await axios.get( json.API_URL + "/pokemon?limit=750", { headers: { 'accept': 'application/json' } });
    this.pokemons = response.data.results;
  }
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 510px;
}
article {
  height: 150px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}

img {
  width: 96px;
  height: 96px;
}
</style>

