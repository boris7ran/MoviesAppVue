<template>
  <div>
    <li>
      ID: {{ movie.id }}
      TITLE: {{ movie.title }}
      DIRECTOR: {{ movie.director }}
      IMAGE: {{ movie.imageURL }}
      DURATION: {{ movie.duration }}
      RELEASE DATE: {{ movie.releaseDate }}
      GENRE: {{ movie.genre }}
    </li>
  </div>
</template>

<script>
export default {
  props: ["movie"]
};
</script>

<style>
</style>
