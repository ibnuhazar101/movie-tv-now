<template>
  <div> 
    <div v-if="dataMovies.length > 0">
      <h1 class="mov-title mt-3 text-right">Now on Cinema</h1>
      <div class="movies">
        <div v-for="(movie, idMovie) in dataMovies" :key="idMovie" class="movie-list" @submit.prevent="" @click="openDetail('movie', movie)">
          <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
          <div class="mov-title">
            {{ movie.title }} ({{ changeDate(movie.release_date) }})
          </div>
        </div>
      </div>
    </div>

    <div v-if="dataTv.length > 0">
      <h1 class="mov-title mt-3 text-right">Now Streaming</h1>
      <div class="movies">
        <div v-for="(tv, idTv) in dataTv" :key="idTv" class="movie-list" @click="openDetail('tv', tv)">
          <img :src="`http://image.tmdb.org/t/p/w500${tv.poster_path}`">
          <div class="mov-title">
            {{ tv.name }} 
          </div>
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
      default: () => []
    },
    dataTv: {
      type: Array,
      default: () => []
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
    openDetail(type, value) {
      this.$router.push(`/detail?type=${type}&typeId=${value.id}`)
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

@media only screen and (max-width: 576px) {
  .movies {
    justify-content: space-around;
  }
  
  .movie-list {
    width: 6.8rem;
  }
}

</style>