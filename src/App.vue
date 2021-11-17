<template>
  <div id="app">
    <!-- Campo di ricerca -->
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div class="input-group my-3">

            <input type="text" class="form-control" placeholder="Inserisci il nome del film..." v-model="filmCercato">

            <button class="btn btn-primary" type="button" id="button-addon2">Cerca</button>

          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <!-- Film -->
          <ul>
            <li v-for="film in listaFilm" :key="film.id">
              {{ film.title }}
            </li>
          </ul>
          <!-- Serie TV -->
          <ul>
            <li v-for="serieTv in listaSerieTv" :key="serieTv.id">
              {{ serieTv.name }}
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
    };
  },
  methods: {
    queryRicerca(url, testoDaCercare, chiave){
      axios.get(this.apiUrl + url, {
      params: {
        api_key: this.apiKey,
        query: testoDaCercare,
        language: "it",
      }
      })
      .then(res => {
        this[chiave] = res.data.results;
      });
    }
  },
  mounted(){
    this.queryRicerca("/search/movie", this.filmCercato, "listaFilm");
    this.queryRicerca("/search/tv", this.filmCercato, "listaSerieTv");
  }
}
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
</style>
