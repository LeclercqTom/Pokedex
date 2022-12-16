<template>
  <div class="searchbar">
    <!-- input correspondant à la barre de recherche  -->
    <input type="text" placeholder="Recherche un pokemon" v-model="recherche">
 </div>

</template>

<script>
 
export default {
  // propriété data
  data() {
    return {
      recherche: "", // variable qui va contenir ce que j'ai saisi dans le input  
    }
  },

  watch: {
    // dès que recherche change 
    recherche: function(newSearch){
      this.$emit("searchPokemonEmit", newSearch); // j'émets un signal relié à la fonction serachPokemonEmit et qui prends la nouvelle recherche 
    }
  }
}
</script>

<style scoped>
.searchbar {
  position: relative;
  width: 100%;
  max-width: 510px;
  padding-bottom: 20px;
}
input {
  border: none;
  outline: none;
  border-radius: 5px;
  padding: 10px 40px 10px 10px;
  width: calc(100% - 50px);
  font-size: 1rem;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
</style>
