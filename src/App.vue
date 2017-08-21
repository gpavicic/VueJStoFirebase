<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js to Firebase</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add a movie</h3>
      </div>

      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addMovie">
          <div class="form-group">
            <label for="movieTitle">Title</label>
            <input type="text" id="movieTitle" class="form-control" v-model="newMovie.title">
          </div>
          <div class="form-group">
            <label for="movieDirector">Director</label>
            <input type="text" id="movieDirector" class="form-control" v-model="newMovie.director">
          </div>
          <div class="form-group">
            <label for="movieURL">URL</label>
            <input type="text" id="movieUrl" class="form-control" v-model="newMovie.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add a movie">
        </form>

      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Movies</h3>
      </div>

      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Director</th>
              <th>Delete</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="movie in movies">
              <td><a target="_blank" v-bind:href="movie.url">{{movie.title}}</a></td>
              <td>{{movie.director}}</td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeMovie(movie)"></span>
              </td>
            </tr>
          </tbody>

        </table>
      </div>
    </div>

  </div>
</template>

<script>

import Firebase from 'firebase'

import toastr from 'toastr'

// access to Firebase
let config = {
    apiKey: "AIzaSyBca5hs2QDsuypNH95KKNek2P_HP4fzXy8",
    authDomain: "vuemovies-9941f.firebaseapp.com",
    databaseURL: "https://vuemovies-9941f.firebaseio.com",
    projectId: "vuemovies-9941f",
    storageBucket: "vuemovies-9941f.appspot.com",
    messagingSenderId: "322135992655"
}

let app = Firebase.initializeApp(config);

// connection to dataBase
let db = app.database();

let moviesRef = db.ref('movies');

export default {
  name: 'app',
  firebase: {
    movies: moviesRef
  },
  data () {
    return {
      newMovie: {
        title: '',
        director: '',
        url: ''
      }
    }
  },
  methods: {
    addMovie: function(){
      moviesRef.push(this.newMovie);
      this.newMovie.title = '';
      this.newMovie.director = '';
      this.newMovie.url = '';
    },
    removeMovie: function(movie){
      moviesRef.child(movie['.key']).remove();
      toastr.success("Movie removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: grey;
  margin-top: 60px;
}
</style>
