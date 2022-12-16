<template>
  <div class="container">
    <!-- Div container comportant les 3 composants du pokedex 
        - La barre de recherche 
        - La liste contenant tous les pokemons
        - Le pop up qui affiche les détails des pokemons
    -->
    <PokemonSearch v-on:searchPokemonEmit="searchPokemon" />
    <PokemonList v-on:ShowPokemonDetail="ShowPokemon" :imageURL="imageURL" :recherche="recherche" />
    <PokemonDetail v-if="showPokemon" v-on:HidePokemonDetail="HidePokemon" :pokemonUrl="pokemonUrl" :imageURL="imageURL" />

  </div>
</template>

<script>
import PokemonDetail from "../components/PokemonDetail.vue";
import PokemonList from "../components/PokemonList.vue";
import PokemonSearch from "../components/PokemonSearch.vue";
import config from "../config/config.json";

export default {
  // Propriété data : 
  data: () => {
    return {
      imageURL: config.IMG_URL, // lien permettant de récupérer l'image de chaque Pokemon
      pokemonUrl: "", // variable permettant de récupérer l'url du pokemon sur lequel on a cliqué
      showPokemon: false, // variable qui permet de savoir si on a cliqué sur un pokemon
      recherche: "", // variable qui récupère la chaine de caractère dans la barre de recherche 
    }
  },
  components: {
    // je déclare les 3 composants pour pouvoir les afficher
    PokemonDetail,
    PokemonList,
    PokemonSearch,
  },
  methods: {

    ShowPokemon(pokemon) {
      // Cette méthode permet (d'afficher) de récupérer les informations sur le pokemon cliqué
      this.showPokemon = true; // j'affiche le pokemon cliqué (affichage du pop up)
      this.pokemonUrl = pokemon; // pokemonUrl stocke le lien de ce pokemon
    },
    HidePokemon() {
      // quand je ferme le pop up 
      this.showPokemon = false; // je passe la variable à false 
      this.pokemonUrl = ""; // je set pokemonUrl à vide
    },

    searchPokemon(recherche) {
      // cette méthode set ma variable recherche à ce que j'ai écrit dans la barre de recherche 
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
  background: radial-gradient(#ffbf0b, #e20000);

  font-family: "Acme", arial;
  font-size: 1rem;
  font-weight: normal;
}

h1 {
  color: #efefef;
}
</style>