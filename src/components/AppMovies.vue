<template>
  <div>
    <ul v-for="movie in movies" :key="movie.id">
      <movie-row :movie="movie" />
    </ul>
  </div>
</template>

<script>
import MovieRow from './MovieRow'
import { moviesService } from '@/services/MoviesService'

export default {
  components: {
    MovieRow
  },

  data() {
    return {
      movies: ""
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
