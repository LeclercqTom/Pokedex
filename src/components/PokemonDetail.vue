<template>
  <div class="detail">
    <div class="detail-view card">
      
      <div class="data card-body">
        
        <img class="image" :src="url + pokemon.name +'.png'" alt="">
        <h2>{{pokemon.name}}</h2>

        <h3>Type(s)</h3>
        <div class="types" >
          <div class="type" v-for="t in pokemon.types" :key="t.id">
            <span :class="t.type.name">{{t.type.name}}</span>
          </div>
        </div>

        <h3>Talents</h3>
        <div class="abilities">
          <div class="ability" v-for="a in pokemon.abilities" :key="a.id">
            {{ a.ability.name }}
        </div>

        </div> 
        <div class="property">
          <div class="left bold">Taille</div>
          <div class="right">{{ pokemon.height}} m</div>
        </div>
        <div class="property">
          <div class="left  bold">Poids</div>
          <div class="right">{{ pokemon.weight}} kg</div>
        </div>

        <div id="evolution">
          
          <div v-if="pokemon.id % 3 === 1">
            <img  :src="url + pokemon.name +'.png'" alt="">
            <img  v-if="pokemon.id <= pokemons.length" :src="url + pokemons[pokemon.id].name +'.png'" alt="">
            <img v-if="pokemon.id+1 <= pokemons.length" :src="url + pokemons[pokemon.id+1].name +'.png'" alt="">
           
            
          </div>
          
          <div v-if="pokemon.id % 3 === 2">
            <img v-if="pokemon.id-2 <= pokemons.length" :src="url + pokemons[pokemon.id-2].name +'.png'" alt="">
            <img  :src="url + pokemon.name +'.png'" alt="">
            <img v-if="pokemon.id <= pokemons.length" :src="url + pokemons[pokemon.id].name +'.png'" alt="">
          </div>

          <div v-if="pokemon.id % 3 === 0">
            <img  :src="url + pokemons[pokemon.id-3].name +'.png'" alt="">
            <img  :src="url + pokemons[pokemon.id-2].name +'.png'" alt="">
            <img  :src="url + pokemon.name +'.png'" alt="">
          </div>

        </div>
       
      </div>
      <button class="close" v-on:click="closeDetail()">Fermer</button>
    </div>
  </div>
</template>

<script>  
// import { config } from 'process';
import axios from '../../node_modules/axios'
import config from '../config/config.json'

export default {
  
  props: ['pokemonUrl',"imageURL",],
  data:() =>{
    return {
      pokemon: [],
      pokemons: [],
      show: false,
      url: config.IMG_URL,
    };
  },
  beforeMount(){
    // appel de l'api pour récuperer la liste des characters
    axios.get(this.pokemonUrl)
      .then((e)=>{
        // sur le retour on stock la data dans caracters
        this.pokemon = e.data;
        this.show = true;
        console.log(this.show);
        console.log(this.pokemon);
      }),
      // appel de l'api pour récuperer la liste des characters
      axios.get(config.API_URL)
      .then((e)=>{
        // sur le retour on stock la data dans caracters
        this.pokemons = e.data.results; 
      })
  },
  
  methods: {
    closeDetail: function(pokemon) {
        console.log('Fermeture');
        this.show = false;
        this.$emit('HidePokemonDetail',pokemon);
      },
  }
};

</script>

<style lang="scss" scoped>
.type {
  .grass {
    background: rgb(3, 139, 44) !important;
  }
  .poison {
    background: rgb(74, 7, 105) !important;
  }
  .water {
    background: rgb(8, 135, 219) !important;
  }
  .dragon {
    background: rgb(27, 2, 68) !important;
  }
  .ice {
    background: rgb(78, 199, 255) !important;
  }
  .flying {
    background: rgb(145, 215, 255) !important;
  }
  .fire {
    background: rgb(238, 135, 17) !important;
  }
  .ghost {
    background: rgb(74, 52, 87) !important;
  }
  .fighting {
    background: rgb(122, 0, 0) !important;
  }
  .normal {
    background: rgb(104, 104, 104) !important;
  }
  .psychic {
    background: rgb(195, 0, 255) !important;
  }
  .bug {
    background: rgb(52, 87, 6) !important;
  }
  .dark {
    background: rgb(43, 43, 43) !important;
  }
  .steel {
    background: rgb(116, 116, 116) !important;
  }
  .fairy {
    background: rgb(248, 165, 237) !important;
  }
  .eletric {
    background: rgb(255, 217, 1) !important;
  }
  .rock {
    background: rgb(88, 95, 100) !important;
  }
  .ground {
    background: rgb(92, 70, 70) !important;
  }
}

.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  //width: calc(100% - 20px);
  // height: calc(100vh - 20px);
  width: 100%;
  height: 100vh;
  background: rgba(10, 7, 0, 0.562);
}
.detail-view {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 90%;
  padding: 50px 0 0;
  position: relative;

  max-width: 510px;

  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
.image {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -60px;
  width: 120px;
  height: 120px;
  background-color: #ffcb04;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}

h2 {
  text-transform: capitalize;
}

.data {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-bottom: 40px;
}
.property {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
  margin-bottom: 10px;
}
.left {
  float: left;
}
.right {
  float: right;
}
h3 {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
}

.types,
.abilities {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 90%;
  max-width: 400px;
}
.type {
  // color: rgb(17, 67, 182);
  margin: 0 0 10px 0;
  padding: 5px 10px;
  font-weight: bold;
  font-size: 1rem;
  letter-spacing: 2px;
  text-transform: capitalize;
  span {
    color: #ffffff !important;
    padding: 10px 14px;
    border-radius: 29px;
  }
}
.ability {
  color: rgb(10, 119, 10);
  margin: 0 10px 10px 0;
  border-radius: 20px;
  padding: 5px 10px;
  font-weight: bold;
  font-size: 1rem;
  letter-spacing: 2px;
  text-transform: capitalize;
  word-wrap: none;
  word-break: keep-all;
  background-color: #ffffff;
  border: 3px solid;
}

.close {
  outline: none;
  border: none;
  border-radius: 5px;
  background-color: #c73015;
  color: #efefef;
  padding: 10px 20px;
  margin-bottom: 20px;
  font-size: 1.2rem;
  cursor: pointer;
}
i {
  font-size: 2rem;
  color: #efefef;
}
.bold {
  font-weight: bold;
}
</style>
