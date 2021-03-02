<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-md-3" id="trending">
        <Trending 
          :dataTrendingTv="TrendingTv"
        />
      </div>
      <div class="col-md-6" id="tv">
        <Tv 
          :dataTv="Tv"
        />
        <div class="d-block btn btn-light mt-3 mb-3" @click="nextPageTv()">
          More
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
      page: '',
      Tv: [],
      TrendingMovie: [],
      TrendingTv: []
    }
  },
  mounted() {
    this.getDataTv()
    this.getDataTrending('tv')
  },
  methods: {
    getDataTv() {
      axios.get(`https://api.themoviedb.org/3/tv/on_the_air?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Tv = res.data.results;
        this.page = res.data.page;
      })
      .catch(err => {
        console.log(err);
      })
    },
    nextPageTv() {
      this.page = this.page+1;
      axios.get(`https://api.themoviedb.org/3/tv/on_the_air?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Tv = [...this.Tv, ...res.data.results];
        return this.Tv;
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
    },
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
  #tv {
    order: 1;
    width: 100%;
  }
  #trending {
    order: 2;
    width: 100%;
  }
}

</style>
