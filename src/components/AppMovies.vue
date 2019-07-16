<template>
  <div>
    <div>
      <movie-search @search-movie="searchMovie" />
    </div>
    <div>
      <p v-if="selectedMovies.length > 0">{{ selectedMovies.length }} Movies are selected</p>
      <button @click="selectAll">Select All</button>
      <button @click="deselectAll">Deselect All</button>
    </div>
    <ul class="list-group" v-for="movie in filteredMovies" :key="movie.id">
      <movie-row :movie="movie" :selectedMovies="selectedMovies" @select-movie="selectMovie" @deselect-movie="deselectMovie" />
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
      selectedMovies: []
    };
  },

  methods: {
    searchMovie(search) {
      this.searchTerm = search;
    },

    selectMovie(id) {
      this.selectedMovies.push(id);
    },

    deselectMovie(id) {
      this.selectedMovies = this.selectedMovies.filter(el => el !== id);
    },

    selectAll() {
      this.selectedMovies = [];
      this.filteredMovies.forEach(movie => {
        this.selectedMovies.push(movie.id);
      })
    },

    deselectAll() {
      this.selectedMovies = [];
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
