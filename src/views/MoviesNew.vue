<template>
  <div class="movies-new">
    <h1>{{ message }}</h1>
    <form @submit.prevent="submitForm">
      <div>
        Title:
        <input type="text" v-model="newMovieTitle" />
        Year:
        <input type="text" v-model="newMovieYear" />
        Plot:
        <input type="text" v-model="newMoviePlot" />
        <!-- <p vif:="!plotIsValid" class="error-message">The Plot is too long.</p>
        Director: -->
        <!-- <input type="text" v-model="newMovieDirector" /> -->
        Character Count:
        <input type="text" v-model="characterCount" />
        <button v-on:click="createMovie">Add a Movie!</button>
      </div>
      <div v-for="movie in movies" v-bind:key="movie">
        <h1>{{ movie.title }}</h1>
        <button v-on:click="showMovie(movie)">More info!</button>
      </div>
    </form>
    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info</h1>
        <p>
          Title:
          <input type="text" v-model="currentMovie.title" />
        </p>
        <p>
          Year:
          <input type="text" v-model="currentMovie.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="currentMovie.plot" />
        </p>
        <p>
          Director:
          <input type="text" v-model="currentMovie.director" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update Movie Info</button>
        <button v-on:click="destroyMovie(currentMovie)">Terminate Movie</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
// import func from "vue-editor-bridge";
export default {
  data: function () {
    return {
      currentMovie: {},
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      errors: [],
    };
  },
  created: function () {},
  methods: {
    // submitForm() {
    //   var plotIsValid = this.form.newMoviePlot.length < 20;
    //   if (plotIsValid) {
    //     console.log("form submitted");
    //   } else {
    //     console.log("invalid form");
    //   }
    // },
    indexMovies: function () {
      axios.get("/api/movies").then((response) => {
        this.movies = response.data;
        console.log("all movies:", this.movies);
      });
    },
    createMovie: function () {
      console.log("adding movie..");
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
      };
      axios
        .post("/api/movies", params)
        .then((response) => {
          console.log("Successfully added movie!", response.data);
          this.movies.push(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
