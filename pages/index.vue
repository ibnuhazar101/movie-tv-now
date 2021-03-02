<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-md-3" id="trending">
        <Trending 
          :dataTrendingMovie="TrendingMovie"
        />
      </div>
      <div class="col-md-6" id="movie">
        <Movie 
          :dataMovies="Movies"
        />
        <div class="d-block btn btn-light mt-3 mb-3" @click="nextPageMovie()">More</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      Movies: [],
      TrendingMovie: [],
      TrendingTv: [],
      page: '',
      movieId: '',
      tvId: ''
    }
  },
  mounted() {
    this.getDataMovies()
    this.getDataTrending('movie')
  },
  methods: {
    getDataMovies() {
      axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Movies = res.data.results;
        this.page = res.data.page;
      })
      .catch(err => {
        console.log(err);
      })
    },
    getDataMoviesDetail() {
      this.movieId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataMovies = res.data
      })
      .catch(err => {
        console.log(err);
      })
    },
    nextPageMovie() {
      this.page = this.page+1;
      axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Movies = [...this.Movies, ...res.data.results];
        return this.Movies;
      })
      .catch(err => {
        console.log(err);
      })
    },
    getDataTrending(value) {
      axios.get(`https://api.themoviedb.org/3/trending/${value}/week?api_key=${this.apiKey}`)
      .then(res => {
        if (value == 'movie') {
          this.TrendingMovie = res.data.results;
        }
        else if (value == 'tv') {
          this.TrendingTv = res.data.results;
        }
      })
      .catch(err => {
        console.log(err);
      })
    }
  }
}
</script>

<style>
body {
  background-color: rgb(26, 47, 66);
}

@media only screen and (max-width: 768px) {
  .row {
    display: flex;
    flex-direction: column;
    padding: 10px;
  }
  #movie {
    order: 1;
    width: 100%;
  }
  #trending {
    order: 2;
    width: 100%;
  }
}

</style>
