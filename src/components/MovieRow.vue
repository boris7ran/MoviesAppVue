<template>
  <div>
    <li class="list-group-item list-group-item-primary" active>
      ID: {{ movie.id }}
      TITLE: {{ movie.title }}
      DIRECTOR: {{ movie.director }}
      IMAGE: {{ movie.imageURL }}
      DURATION: {{ movie.duration }}
      RELEASE DATE: {{ movie.releaseDate }}
      GENRE: {{ movie.genre }}
      <button v-if="!selected" @click="selectMovie">Select</button>
      <button v-else @click="deselectMovie">Deselect</button>
    </li>
  </div>
</template>

<script>
export default {
  props: ["movie"],

  data() {
    return{
      selected: false
    }
  },

  methods: {
    selectMovie(){
      this.$emit('select-movie');
      this.selected = true;
    },

    deselectMovie(){
      this.$emit('deselect-movie');
      this.selected = false;
    }
  }
};
</script>

<style>
</style>
