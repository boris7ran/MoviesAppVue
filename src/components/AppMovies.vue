<template>
  <div>
    <div>
      <movie-search @search-movie="searchMovie" />
    </div>
    <div>
      <p v-if="selectedMovies > 0">{{ selectedMovies }} Movies have been selected</p>
    </div>
    <ul ul class="list-group" v-for="movie in filteredMovies" :key="movie.id">
      <movie-row :movie="movie" @select-movie="selectMovie" @deselect-movie="deselectMovie" />
    </ul>
    <div>
      <p v-if="filteredMovies.length === 0">No movies found</p>
    </div>
  </div>
</template>

<script>
import MovieRow from "./MovieRow";
import { moviesService } from "@/services/MoviesService";
import MovieSearch from "./MovieSearch";

export default {
  components: {
    MovieRow,
    MovieSearch
  },

  data() {
    return {
      movies: [],
      searchTerm: "",
      selectedMovies: 0
    };
  },

  methods: {
    searchMovie(search) {
      this.searchTerm = search;
    },

    selectMovie() {
      this.selectedMovies++;
    },

    deselectMovie() {
      this.selectedMovies--;
    }
  },

  computed: {
    filteredMovies() {
      return this.movies.filter(movie =>
        movie.title.toLowerCase().includes(this.searchTerm.toLowerCase())
      );
    }
  },

  beforeRouteEnter(to, from, next) {
    next(vm => {
      moviesService
        .getAll()
        .then(response => {
          vm.movies = response.data;
        })
        .catch(error => {
          alert(error);
        });
    });
  }
};
</script>

<style>
</style>
