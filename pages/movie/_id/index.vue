<template>
  <div class="back mt-1">
    <div class="container mt-3 mb-3">
      <div class="row justify-content-center">
        <div class="col-4">
          <img class="poster" :src="`https://image.tmdb.org/t/p/w500/${dataMovies.poster_path}`">
        </div>
        <div class="col-6">
          <h1 class="bold"> {{ dataMovies.title }} </h1>
          Release Date: <span class="bold">{{ dataMovies.release_date }}</span><br>
          Genre: 
          <div v-for="(genre, idGenreMovie) in dataMovies.genres" :key="idGenreMovie" class="d-inline bold"> {{ genre.name }},</div>-<br>
          Run Time: <span class="bold">{{ dataMovies.runtime }}m</span> <br>
          <br>
          <p>
            {{ dataMovies.overview }}
          </p>
          <hr>
          Production Studio: <br>
          <div v-for="(studio, idStudioMovie) in dataMovies.production_companies" :key="idStudioMovie" class="d-inline bold"> {{ studio.name }},</div>-<br>
          Stars: <br>
          <div v-for="(stars, idStarMovie) in creditCast.slice(0, 10)" :key="idStarMovie" class="d-inline bold"> {{ stars.name }},</div>-<br>
          <hr>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      movieId: '',
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataMovies: [],
      dataMovieCredit: [],
      creditCast: []
    }
  },
  mounted() {
    this.getDataMovies()
  },
  methods: {
    getDataMovies() {
      this.movieId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataMovies = res.data
        console.log(res);
      })
      .catch(err => {
        console.log(err);
      }),

      axios.get(`https://api.themoviedb.org/3/movie/${this.movieId}/credits?api_key=${this.apiKey}`)
      .then(res => {
        this.dataMovieCredit = res.data
        this.creditCast = res.data.cast
        console.log(res);
      })
      .catch(err => {
        console.log(err);
      })
    } 
  }
}
</script>

<style>
.back {
  background-color: white;
  padding: 10px 0;
}

.bold {
  font-weight: bold;
}

.poster {
  width: 100%;
  border-radius: 10px;
}
</style>