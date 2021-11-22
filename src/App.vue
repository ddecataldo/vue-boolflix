<template>
  <div id="app">

    <nav class="navbar navbar-light bg-dark">
      <div class="container-fluid">
        <a class="navbar-brand">Navbar</a>
        <form class="d-flex">
          <input type="text" class="form-control me-2" placeholder="Inserisci il nome del film..." v-model="filmCercato">
          <button class="btn btn-danger" type="button" @click="queryRicerca">Cerca</button>
        </form>
      </div>
    </nav>

    <div class="container my-5">
      <div class="row mb-5">
        <div class="col-md-4 mb-3" v-for="film in listaFilm" :key="film.id">
          <div class="card">
            <img class="my_thumb" :src="generaThumb(film.poster_path)">
            <div class="dati_card">
              <p><strong>Titolo</strong>: {{ film.title }}</p>
              <p><strong>Titolo originale </strong>{{ film.original_title }}</p> 
              <p><strong>Lingua</strong> <img :src="getFlagPath(film.original_language)"></p>
              <p><i
                v-for="i in 5"
                :key="'vote_star_' + i"
                class="fa"
                :class="i <= generaStelle ? 'fa-star' : 'fa-star-o'"
                ></i></p>
            </div>  
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4 mb-3" v-for="serieTv in listaSerieTv" :key="serieTv.id">
          <div class="card">
              <img class="my_thumb" :src="generaThumb(serieTv.poster_path)">
              <p><strong>Titolo</strong>: {{ serieTv.name }}</p>
              <p><strong>Titolo originale </strong>{{ serieTv.original_name }}</p>
              <p><strong>Lingua</strong> <img :src="getFlagPath(serieTv.original_language)"></p> 
              <p><i
                v-for="i in 5"
                :key="'vote_star_' + i"
                class="fa"
                :class="i <= generaStelle ? 'fa-star' : 'fa-star-o'"
                ></i></p> 
            </div>
          </div>
        </div>
      </div>
    </div>

          <!-- Film -->
          <!-- <h1>Film</h1>
          <ul>
            <li v-for="film in listaFilm" :key="film.id">
              <img :src="generaThumb(film.poster_path)"> - 
              {{ film.title }} - 
              {{ film.original_title }} - 
              <img :src="getFlagPath(film.original_language)"> - 
              <i
                v-for="i in 5"
                :key="'vote_star_' + i"
                class="fa"
                :class="i <= generaStelle ? 'fa-star' : 'fa-star-o'"
                ></i>
              <hr>
            </li>
          </ul> -->
          <!-- Serie TV -->
          <!--h1>Serie TV</h1>
          <ul>
            <li v-for="serieTv in listaSerieTv" :key="serieTv.id">
              <img :src="generaThumb(serieTv.poster_path)">
              {{ serieTv.name }} - 
              {{ serieTv.original_name }} - 
              <img :src="getFlagPath(serieTv.original_language)"> - 
              <i
                v-for="i in 5"
                :key="'vote_star_' + i"
                class="fa"
                :class="i <= generaStelle ? 'fa-star' : 'fa-star-o'"
                ></i>
              <hr>
            </li>
          </ul> --> 
        

</template>

<script>

import axios from "axios";

export default {
  name: 'App',
  components: {},
  data() {
    return {
      apiKey: "5d72883ce5a456a4913de11d19f67e70",
      apiUrl: "https://api.themoviedb.org/3",
      imageUrl: "https://image.tmdb.org/t/p/",
      sizeImage: "w342",
      filmCercato: "",
      listaFilm: [],
      listaSerieTv: [],
      lingua: {
        en: "england.png",
        it: "italy.png",
      },
    };
  },
  methods: {
    queryRicerca(){
      this.queryRicercaFilm();
      this.queryRicercaSerie();
    },
    queryRicercaFilm(){
      axios.get(this.apiUrl + '/search/movie', {
      params: {
        api_key: this.apiKey,
        query: this.filmCercato,
        language: "it",
      }
      })
      .then(res => {
        this.listaFilm = res.data.results;
      });
    },
    queryRicercaSerie(){
      axios.get(this.apiUrl + '/search/tv', {
      params: {
        api_key: this.apiKey,
        query: this.filmCercato,
        language: "it",
      }
      })
      .then(res => {
        this.listaSerieTv = res.data.results;
      });
    },
    getFlagPath(lang){
      if(!this.lingua[lang]){
        return require('@/assets/flags/noflags.png');
      }

      return require('@/assets/flags/' + this.lingua[lang]);
    },
    generaThumb(path){
      if(!path){
        return require('@/assets/placeholder.jpg');
      }
      return this.imageUrl + this.sizeImage + path;
    },
    generaStelle() {
      const voto = this.data.vote_average;
      const votoFinale = Math.round(voto / 2);
      return votoFinale;
    },
  },
  mounted(){
    /* this.queryRicerca("/search/movie", "this.filmCercato", "listaFilm");
    this.queryRicerca("/search/tv", "this.filmCercato", "listaSerieTv"); */
  }
}
</script>

<style lang="scss">
@import "~font-awesome/css/font-awesome.css";
@import "~bootstrap/scss/bootstrap";
body{
  background-color: #555555;
}
.card img.my_thumb{
  width: 100%;
}
.dati_card{
  display: none;
}
.card:hover::before {
    background-color: #000;
    content: " ";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
.card:hover .dati_card{
  display: block;
  position: absolute;
  top: 10px;
  left: 10px;
  color:#fff;
}
</style>
