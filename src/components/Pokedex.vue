<template>
  <div class="container">
    <PokemonSearch @searchPokemonEmit = "setPokemonSearch"/>
    <PokemonDetail @closePokemon = "unselectPokemon" v-bind:pokemonUrl = "selectedPokemon" :imgurl = "pokeImgUrl"/>
    <PokemonList @showpokemon = "selectPokemon" v-bind:searchedPoke="pokemonSearch"/>
  </div>
</template>

<script>
import PokemonDetail from "../components/PokemonDetail.vue";
import PokemonList from "../components/PokemonList.vue";
import PokemonSearch from "../components/PokemonSearch.vue";
import json from "../config/config.json";

export default {
  components: {
    PokemonDetail,
    PokemonList,
    PokemonSearch,
  },
  data() {
    return {
      selectedPokemon: "",
      pokeImgUrl: "",
      pokemonSearch: ""
    }
  },
  methods: {
    selectPokemon( pokemon ) {
      // Récupération des pok"mons séléctionés pour afficher les détails
      this.selectedPokemon = pokemon.url;
      this.pokeImgUrl = json.IMG_URL + pokemon.name + '.png';
    },
    unselectPokemon() {
      // On cache les pokemon non-visibles
      this.selectedPokemon = "";
      this.pokeImgUrl = "";
    },
    setPokemonSearch( searchBarContent ) {
      this.pokemonSearch = searchBarContent;
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 10px;
  width: calc(100% - 20px);
  min-height: calc(100vh - 20px);
  //background: radial-gradient(#ffbf0b, #e20000);

  font-family: "Acme", arial;
  font-size: 1rem;
  font-weight: normal;
}

h1 {
  color: #efefef;
}
</style>