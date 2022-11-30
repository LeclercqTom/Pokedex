<template>
  <div class="container">
    <PokemonSearch v-on:searchPokemonEmit="searchPokemon"/>
    <PokemonList v-on:ShowPokemonDetail="ShowPokemon" :imageURL="imageURL" :recherche="recherche"/>
    <PokemonDetail v-if="showPokemon" v-on:HidePokemonDetail="HidePokemon" :pokemonUrl="pokemonUrl" :imageURL="imageURL" />
    
  </div>
</template>

<script>
import PokemonDetail from "../components/PokemonDetail.vue";
import PokemonList from "../components/PokemonList.vue";
import PokemonSearch from "../components/PokemonSearch.vue";
import config from "../config/config.json";

export default {
  data: ()=>{
    return{
      imageURL: config.IMG_URL,
      pokemonUrl: "",
      showPokemon: false,
      recherche: "",
    }
  },
  components: {
    PokemonDetail,
    PokemonList,
    PokemonSearch,
  },
  methods: {
    ShowPokemon(pokemon){
      this.showPokemon = true;
      this.pokemonUrl = pokemon;
      // console.log(this.pokemonUrl);
      
      // console.log(pokemon);
      console.log("Show pokemon");
    },
    HidePokemon(){
      this.showPokemon = false;
      this.pokemonUrl = "";
      console.log("Hide pokemon");
    },

    searchPokemon(recherche){
      this.recherche = recherche;
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