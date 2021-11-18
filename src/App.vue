<template>
  <div id="app">
    <!-- Campo di ricerca -->
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div class="input-group my-3">

            <input type="text" class="form-control" placeholder="Inserisci il nome del film..." v-model="filmCercato">

            <button class="btn btn-primary" type="button" id="button-addon2" @click="queryRicerca">Cerca</button>

          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <!-- Film -->
          <h1>Film</h1>
          <ul>
            <li v-for="film in listaFilm" :key="film.id">
              {{ film.title }} - 
              {{ film.original_title }} - 
              <img :src="getFlagPath(film.original_language)"> - 
              {{ film.vote_count }}
              <hr>
            </li>
          </ul>
          <!-- Serie TV -->
          <h1>Serie TV</h1>
          <ul>
            <li v-for="serieTv in listaSerieTv" :key="serieTv.id">
              {{ serieTv.name }} - 
              {{ serieTv.original_name }} - 
              <img :src="getFlagPath(serieTv.original_language)"> - 
              {{ serieTv.vote_count }}
              <hr>
            </li>
          </ul>
        </div>
      </div>   
    </div>
  </div>  
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
    }
  },
  mounted(){
    /* this.queryRicerca("/search/movie", "this.filmCercato", "listaFilm");
    this.queryRicerca("/search/tv", "this.filmCercato", "listaSerieTv"); */
  }
}
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
</style>
