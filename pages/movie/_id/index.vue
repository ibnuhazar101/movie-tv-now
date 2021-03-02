<template>
  <div class="back mt-1" v-if="dataMovies">
    <div class="container mt-3 mb-3">
      <div class="row justify-content-center">
        <div class="col-md-4">
          <img class="poster" :src="`https://image.tmdb.org/t/p/w500/${dataMovies.poster_path}`">
        </div>
        <div class="col-md-6">
          <h1 class="bold"> {{ dataMovies.title }} </h1>
          <div class="info">
            Release Date: <span class="bold">{{ dataMovies.release_date }}</span><br>
            Genre: <span class="bold">{{ setNames(dataMovies.genres) }}-</span> <br>
            Run Time: <span class="bold">{{ dataMovies.runtime }}m</span> <br>
          </div>
          <br>
          <div class="overview">
            <p>{{ dataMovies.overview }}</p>
          </div>
          <hr>
          <div class="studio">
            Production Studio:
            <p class="bold">
              {{ setNames(dataMovies.production_companies) }}-
            </p>
            Stars: <br>
            <p class="bold">
              {{ setNames(creditCast) }}-
            </p>
          </div>
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
      creditCast: []
    }
  },
  mounted() {
    this.getDataMovies()
    this.getCast()
  },
  methods: {
    setNames(arrayNames) {
      let names = ''
      arrayNames && arrayNames.forEach(element => {
        names += ` ${element.name},`
      });
      return names
    },
    getDataMovies() {
      this.movieId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataMovies = res.data
        console.log(this.dataMovies);
      })
      .catch(err => {
        console.log(err);
      })
    },
    getCast() {
      axios.get(`https://api.themoviedb.org/3/movie/${this.movieId}/credits?api_key=${this.apiKey}`)
      .then(res => {
        this.creditCast = res.data.cast.slice(0, 10)
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

@media only screen and (max-width: 768px) {
  .poster {
    width: 50%;
    margin-bottom: 10px;
  }
}

</style>