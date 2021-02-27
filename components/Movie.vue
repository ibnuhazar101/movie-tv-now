<template>
  <div class=""> 
    <h1 class="mov-title mt-3 text-right">Now on Cinema</h1>
    <div class="movies">
      <div v-for="(movie, idMovie) in dataMovies" :key="idMovie" class="movie-list" @click="openDetail(movie)">
        <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
        <div class="mov-title">
          {{ movie.title }} ({{ changeDate(movie.release_date) }})
        </div>
      </div>
    </div>
    <div class="d-block btn btn-light mt-3 mb-3" @click="nextPage()">More</div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataMovies: [],
      page: ''
    }
  },
  mounted() {
    this.getDataMovies()
  },
  methods: {
    getDataMovies() {
      axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.dataMovies = res.data.results;
        this.page = res.data.page;
        console.log(this.dataMovies);
      })
    },
    changeDate(date) {
      var d = new Date(date);
      var n = d.getFullYear();
      return n;
    },
    nextPage() {
      this.page = this.page+1;
      axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        console.log(res.data.page);
        this.dataMovies = [...this.dataMovies, ...res.data.results];
        return this.dataMovies;
      })
    },
    openDetail(movie) {
      this.$router.push(`/movie/${movie.id}`)
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
  width: 8.5rem;
}

.mov-title {
  color: white;
  /* font-weight: bold; */
}

</style>