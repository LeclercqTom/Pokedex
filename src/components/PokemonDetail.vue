<template>
  <!-- Cette div contient tout le pop up -->
  <div class="detail">

    <div class="detail-view card">

      <div class="data card-body">
        <!-- Dans le corps du pop up j'affiche :
          - L'image qui correspond au pokemon
          - Le nom du pokemon
          - Les types du pokemon
          - Ses talents
          - Sa taille et son poids
          - Sa lignée d'évolution
        -->

        <!-- L'image et le nom du pokemon -->
        <img class="image" :src="url + pokemon.name + '.png'" alt="">
        <h2>{{ pokemon.name }}</h2>

        <!-- Les types -->
        <h3>Type(s)</h3>
        <!-- Je créé une div qui va contenir tous les types -->
        <div class="types"> 
          <!-- Pour chaque type -->
          <div class="type" v-for="t in pokemon.types" :key="t.id">
            <!-- je mets le nom du type comme classe pour avoir le bon css -->
            <!-- J'affiche le nom du type -->
            <span :class="t.type.name">{{ t.type.name }}</span>
          </div>
        </div>

        <!-- Les talents -->
        <h3>Talents</h3>
        <div class="abilities">
          <!-- Pour chaque talent -->
          <div class="ability" v-for="a in pokemon.abilities" :key="a.id">
            <!-- J'affiche le talent correspondant au pokemon -->
            {{ a.ability.name }} 
          </div>

        </div>
        <div class="property">
          <!-- J'affiche la taille du pokemon -->
          <div class="left bold">Taille</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <!-- J'affiche le poids du pokemon -->
          <div class="left  bold">Poids</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>

        <div id="evolution">
          <div>
            <!-- J'affiche l'image correspondant au pokemon de base-->
            <img :src="url + pokemonEvo.chain.species.name + '.png'" alt="">

            <!-- Si le pokemon possède un évolution j'affiche l'image de celle-ci -->
            <img v-if="pokemonEvo.chain.evolves_to.length > 0" :src="url + pokemonEvo.chain.evolves_to[0].species.name + '.png'" alt="" >

            <!-- Si le pokemon possède une deuxième évolution j'affiche également l'image de celle-ci -->
            <img v-if="pokemonEvo.chain.evolves_to[0].evolves_to.length > 0" :src="url + pokemonEvo.chain.evolves_to[0].evolves_to[0].species.name + '.png'" alt="">
          </div>
        </div>
      </div>
      <!-- bouton permettant de fermer le pop up -->
      <button class="close" v-on:click="closeDetail()">Fermer</button>
    </div>
  </div>
</template>

<script>
import axios from '../../node_modules/axios'
import config from '../config/config.json'

export default {
  // Les props permettent de transférer les données pokemonUrl et imageURL dans ce composant
  props: ['pokemonUrl', "imageURL",],
  data () {
    return {
      pokemon: [], // récupére les infos d'un pokemon
      pokemonInfo: [], // récupère des informations complémentaires sur le pokemon
      pokemonEvo: [], // récupère des informations sur la lignée d'évolution
      show: false, 
      url: config.IMG_URL, // récupère le lien du JSON

    };
  },
  // appelé juste avant que le composant soit créé
  beforeMount() {
    // appel pour récuperer les infos d'un pokemon
    axios.get(this.pokemonUrl)
      .then((e) => {
        // je stocke ces infos dans la variable pokemon
        this.pokemon = e.data;
        // si je récupère ces infos cela veut dire que j'ai ouvet le pop up donc je passe à true
        this.show = true; 

        // appel pour récuperer des infos supplémentaires sur le pokemon
        axios.get("https://pokeapi.co/api/v2/pokemon-species/" + this.pokemon.name)
          .then((e) => {
            // je stock ces infos dans la variable pokemonInfo
            this.pokemonInfo = e.data;

            // appel pour récuperer des infos sur la lignée d'évolution 
            axios.get(this.pokemonInfo.evolution_chain.url)
              .then((e) => {
                // je stocke ces infos dans la varaible pokemonEvo
                this.pokemonEvo = e.data;
              })
          })
      })
  },

  methods: {
    // Cette méthode envoie un signal quand je clique sur le bouton fermer
    closeDetail: function (pokemon) {
      this.show = false; // je passe show à false
      this.$emit('HidePokemonDetail', pokemon); // La fonction s'appelle HidePokemonDetail et je récupère le pokemon
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
