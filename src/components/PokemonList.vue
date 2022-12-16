<template>
  <!-- Div contenant tous les pokemons -->
  <div class="list">
    <!-- Pour chaque pokemon dans la fonctionpokemonFiltered + dès que je clique ça prend le lien du pokemon-->
    <article v-for="pokemon in pokemonsFiltered" v-bind:key="pokemon.name" v-on:click="showPokemonDetail(pokemon.url)">
      <!-- J'affiche l'image et le nom du pokemon -->
      <img :src="imageURL + pokemon.name +'.png'" alt="">
      <h3>{{pokemon.name}}</h3>
    </article>
  </div> 
</template>

<script>

import axios from '../../node_modules/axios'
import config from '../config/config.json'

export default {
  // appelé juste avant que le composant soit créé
  beforeMount(){
    // appel pour récupérer tous les pokemons
    axios.get(config.API_URL)
      .then((e)=>{
        // Je stocke tout dans la variable pokemons
        this.pokemons = e.data.results;
      })
  } ,   
  // Les props permettent de transférer les données pokemonUrl et imageURL dans ce composant
  props:['imageURL',"recherche"],
  // propriété data 
  data: function () {
    return {
      pokemons: [] // variable qui récupère tous les pokemons
    };
  },
  methods:{
    // Cette méthode envoie un signal quand je clique sur un pokemon pour vouloir l'afficher
    showPokemonDetail: function(pokemon){
      this.$emit("ShowPokemonDetail",pokemon); // relier à la fonction ShowPokemonDetail + récupère le pokemon en question
    }
  },
  
  computed: {
    // fonction qui permet de filtrer la liste de pokemon
    pokemonsFiltered: function(){ 
      // si ma recherche est vide    
      if (this.recherche === ""){
        // j'affiche tous les pokemons
        return this.pokemons
      }
      // sinon
      else{
        // Je filtre selon ma recherche
        let filteredList = this.pokemons.filter((pokemon) => {
        return pokemon.name.includes(this.recherche);
       })
       return filteredList
      }
    }     
  }
}

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

