<template>
  <div>
    <div>
      <movie-search @search-movie="searchMovie"/>
    </div>
    <ul v-for="movie in filteredMovies" :key="movie.id">
      <movie-row :movie="movie" />
    </ul>
  </div>
</template>

<script>
import MovieRow from './MovieRow'
import { moviesService } from '@/services/MoviesService'
import MovieSearch from './MovieSearch'

export default {
  components: {
    MovieRow,
    MovieSearch
  },

  data() {
    return {
      movies: [],
      searchTerm: ""
    }
  },

  methods: {
    searchMovie(search){
      this.searchTerm = search;
        

    }
  },

  computed: {
    filteredMovies() {
      return this.movies.filter( movie => movie.title.toLowerCase().includes(this.searchTerm.toLowerCase()));
    }
  },

  beforeRouteEnter(to, from, next) {
    next(vm => {
      moviesService.getAll()
        .then(response => {
          vm.movies = response.data;
        }).catch(error => {
          alert(error);
        })
    })
  }
}
</script>

<style>
</style>
