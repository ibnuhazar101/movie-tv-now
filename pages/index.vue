<template>
  <div>

    <div class="row justify-content-center">
      <div class="col-md-4 col-lg-3" id="trending">
        <Trending 
          :dataTrendingMovie="TrendingMovie"
        />
      </div>
      <div class="col-md-7 col-lg-6" id="movie">
        <ContentList 
          :dataMovies="Movies"
        />
        <div class="d-block btn btn-light mt-3 mb-3" @click="setPage()">More</div>
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
      Search: [],
      page: 1
    }
  },
  watch: {
    page() {
      this.getDataMovies()
    }
  },
  mounted() {
    this.getDataMovies()
    this.getTrendingMovie()
  },
  methods: {
    getDataMovies() {
      axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Movies = [...this.Movies, ...res.data.results]
      })
      .catch(err => {
        console.log(err);
      })
    },
    setPage() {
      this.page += 1
    },
    getTrendingMovie() {
      axios.get(`https://api.themoviedb.org/3/trending/movie/week?api_key=${this.apiKey}`)
      .then(res => {
        this.TrendingMovie = res.data.results;
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

@media only screen and (max-width: 576px) {
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
