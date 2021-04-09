<template>
  <div id="app">
    <img alt="Movies logo" src="./assets/logo.png">
    <div id="conteneur">
      <!-- filtrage -->  
      <div id="filtre">
        <select class="btn btn-dark" v-model="selected">
          <option disabled value="">Category</option>
          <option value="all">All Films</option>
          <option v-for="option in options" :key="option.id" :value="option">{{option}}</option>
        </select>
      </div>
      <!-- affichage des films -->
      <div id="affiche" class="row">
        <div v-for="(movie,index) in newData" :key="index" class="card" style="width: 18rem;">
          <img :src='movie.image' class="card-img-top" alt="film poster image">
          <div class="card-body">
            <h5 class="card-title">{{movie.title}}</h5>
            <div id="note">
              <p class="text-primary"><font-awesome-icon :icon="['far', 'thumbs-up']" />{{movie.likes}}</p>
              <p class="text-danger"><font-awesome-icon :icon="['far', 'thumbs-down']" />{{movie.dislikes}}</p>
            </div>
            <p class="card-text">Category: {{movie.category}}</p>
            <!-- Les v-if sont présent pour assurer le toggle -->
            <a v-if="movie.clickdislike === true" class="btn btn-primary" v-on:click="like(movie.id, movie.likes, movie.dislikes)"><font-awesome-icon :icon="['far', 'thumbs-up']" /></a>
            <a v-if="movie.clickdislike === false" class="btn btn-danger" v-on:click="dislike(movie.id, movie.dislikes, movie.likes)"><font-awesome-icon :icon="['far', 'thumbs-down']" /></a>
            <a class="btn btn-warning delButton" v-on:click="del(index)">DELETE</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import { movies } from './movies.js'
export default {
  name: 'App',
  data(){
    return {
      movieData : movies,
      selected:"",
    }
  },
  created: async function(){ //lors de la creation injection d'une donnée dans le tableau movieData pour le toggle
    for (let i = 0; i < this.movieData.length; i++){
      this.movieData[i].clickdislike = true; 
    }
  },
  methods:{
    like(idselect,valuelike,valuedislike){ //incrémentation like
      this.movieData.forEach(function(obj) {
        if (obj.id === idselect){
          if(obj.click === true){
            obj.dislikes = valuedislike-1; //décrémentation du dislike si le click à déja été fait sur le dislike
          }
          obj.likes = valuelike+1;
          obj.clickdislike = false;
        }
      })
    },
    dislike(idselect2,valuedis,valuelike){ // incrémentation dislike
      this.movieData.forEach(function(obj) {
        if (obj.id === idselect2){
          if(obj.clickdislike === false){
            obj.likes = valuelike-1; //décrémentation du like
          }
          obj.dislikes = valuedis+1;
          obj.click = true;
          obj.clickdislike = true;
        }
      })
    },
    del(index){ //suppression du film
      this.movieData.splice(index, 1)
      console.log(index);
    }
  },
  computed:{
    options(){ // filtrer les category
      var tableau = [];
      for(let i = 0; i < this.movieData.length; i++){
        if(tableau.includes(this.movieData[i].category)){
          continue;
        }
        tableau.push(this.movieData[i].category)
      }
      return tableau;
    },
    newData(){ // affichage des films suivant le filtrage
      var newTabs = this.movieData;
      if(this.selected != "all"){
      return newTabs.filter(newTab => !newTab.category.indexOf(this.selected));
      } else {
        return newTabs;
      }
    } 
  },
};

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
#affiche{
  display: flex;
  justify-content: center;
}
.card{
  margin:20px;
}
#note{
  display: flex;
  justify-content: center;
}
#note>p{
  margin:10px;
}
a.delButton{
  margin:10px;
}
#conteneur{
  margin-top: 30px;
}
</style>
