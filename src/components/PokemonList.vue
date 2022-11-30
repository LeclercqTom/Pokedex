<template>
  <div class="list">
    <article v-for="pokemon in pokemonsFiltered" v-bind:key="pokemon.name" v-on:click="showPokemonDetail(pokemon.url)">
      <img :src="imageURL + pokemon.name +'.png'" alt="">
      <h3>{{pokemon.name}}</h3>
    </article>
  </div> 
</template>

<script>

// import d'axios pour faire une requete http
import axios from '../../node_modules/axios'
import config from '../config/config.json'
// import _ from "lodash";

export default {
  // Called right before the component is to be mounted.
  beforeMount(){
    // appel de l'api pour récuperer la liste des characters
    axios.get(config.API_URL)
      .then((e)=>{
        // sur le retour on stock la data dans caracters
        this.pokemons = e.data.results;
        console.log(this.pokemons);
      })
  } ,   
  props:['imageURL',"recherche"],
  data: function () {
    return {
      // datastore
  
      pokemons: []
    };
  },
  methods:{
    showPokemonDetail: function(pokemon){
      console.log('open');
      console.log(pokemon);
      this.$emit("ShowPokemonDetail",pokemon);
    }
  },
  computed: {
    pokemonsFiltered: function(){     
      if (this.recherche === ""){
        return this.pokemons
      }
      else{
        let filteredStories = this.pokemons.filter((pokemon) => {
        return pokemon.name.includes(this.recherche);
       })
       return filteredStories
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

