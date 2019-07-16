<template>
  <div>
    <div>
      <movie-search @search-movie="searchMovie" />
    </div>
    <div>
      <p v-if="selectedMovies.length > 0">{{ selectedMovies.length }} Movies are selected</p>
      <div class="btn-group" role="group" aria-label="Basic example">
        <button type="button" class="btn btn-secondary" @click="selectAll">Select All</button>
        <button type="button" class="btn btn-secondary" @click="deselectAll">Deselect All</button>
        <button type="button" class="btn btn-secondary" @click="sortByNameAsc">Name Asc</button>
        <button type="button" class="btn btn-secondary" @click="sortByNameDesc">Name Desc</button>
        <button type="button" class="btn btn-secondary" @click="sortByDurationAsc">Duration Asc</button>
        <button type="button" class="btn btn-secondary" @click="sortByDurationDesc">Duration Desc</button>
      </div>
    </div>
    <ul class="list-group" v-for="movie in filteredMovies" :key="movie.id">
      <movie-row :movie="movie" :selected="isSelected(movie.id)" @select-movie="selectMovie" @deselect-movie="deselectMovie" />
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
    },

    isSelected(id) {
      return this.selectedMovies.includes(id);
    },

    sortByNameAsc() {
      this.movies.sort(this.sortByName);
    },

    sortByNameDesc() {
      this.movies.sort(this.sortByName).reverse();
    },

    sortByDurationAsc() {
      this.movies.sort(this.sortByDuration);
    },

    sortByDurationDesc() {
      this.movies.sort(this.sortByDuration).reverse();
    },

    sortByName(a, b) {
      let titleA = a.title.toUpperCase();
      let titleB = b.title.toUpperCase();

      if (titleA > titleB){
        return 1;
      }
      if (titleA < titleB){
        return -1
      }
      
      return 0;
    },

    sortByDuration(a, b) {
      if (a.duration > b.duration){
        return 1;
      }
      if (a.duration < b.duration){
        return -1
      }
      
      return 0;
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
