<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-md-3" id="trending">
        <Trending />
      </div>
      <div class="col-md-6" id="movie">
        <div> 
          <h1 class="mov-title mt-3 text-right">Search Results</h1>
          <div class="movies">
            <div v-for="(movie, idMovie) in dataMovies" :key="idMovie" class="movie-list" @click="openDetail(movie)">
              <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
              <div class="mov-title">
                <div v-if="movie.media_type == 'movie'">
                  {{ movie.title }} ({{ changeDate(movie.release_date) }})
                </div>
                <div v-else-if="movie.media_type == 'tv'">
                  {{ movie.name }}
                </div>
                <div v-else>
                  NaN
                </div>
              </div>
            </div>
          </div>
          <div class="d-block btn btn-light mt-3 mb-3" @click="nextPage()">More</div>
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
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataMovies: [],
      page: '',
      searchId: ''
    }
  },
  mounted() {
    this.getDataMovies()
  },
  methods: {
    getDataMovies() {
      this.searchId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/search/multi?api_key=${this.apiKey}&query=${this.searchId}&page=${this.page}`)
      .then(res => {
        this.dataMovies = res.data.results;
        this.page = res.data.page;
      })
    },
    changeDate(date) {
      var d = new Date(date);
      var n = d.getFullYear();
      return n;
    },
    nextPage() {
      this.page = this.page+1;
      axios.get(`https://api.themoviedb.org/3/search/multi?api_key=${this.apiKey}&query=${this.searchId}&page=${this.page}`)
      .then(res => {
        this.dataMovies = [...this.dataMovies, ...res.data.results];
        return this.dataMovies;
      })
    },
    openDetail(movie) {
      if (movie.media_type == 'movie') {
        this.$router.push(`/movie/${movie.id}`)
      }
      else {
        this.$router.push(`/tv/${movie.id}`)
      }
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