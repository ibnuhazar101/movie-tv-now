<template>
  <div> 
    <h1 class="mov-title mt-3 text-right">Now on Cinema</h1>
    <div class="movies">
      <div v-for="(movie, idMovie) in dataMovies" :key="idMovie" class="movie-list" @click="openDetail(movie)">
        <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
        <div class="mov-title">
          {{ movie.title }} ({{ changeDate(movie.release_date) }})
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    dataMovies: {
      type: Array,
      default: []
    },
    page: Number
  },
  data() {
    return {
    }
  },
  methods: {
    changeDate(date) {
      var d = new Date(date);
      var n = d.getFullYear();
      return n;
    },
    openDetail(movie) {
      this.$router.push(`/detail?type=movie&id=${movie.id}`)
    }
  }
}
</script>

<style>

.movies {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0 auto;
}

.movie-list {
  width: 8.5rem;
  margin-bottom: 1rem;
  cursor: pointer;
}

.movie-list img {
  width: 100%;
}

.mov-title {
  color: white;
}

</style>