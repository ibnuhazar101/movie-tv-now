<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-md-4 col-lg-3" id="trending">
        <Trending 
          :dataTrendingTv="TrendingTv"
        />
      </div>
      <div class="col-md-7 col-lg-6" id="tv">
        <ContentList 
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
      page: 1,
      Tv: [],
      TrendingTv: []
    }
  },
  mounted() {
    this.getDataTv()
    this.getTrendingTv()
  },
  methods: {
    getDataTv() {
      axios.get(`https://api.themoviedb.org/3/tv/on_the_air?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.Tv = [...this.Tv, ...res.data.results];
      })
      .catch(err => {
        console.log(err);
      })
    },
    nextPageTv() {
      this.page = this.page+1
      this.getDataTv()
    },
    getTrendingTv() {
      axios.get(`https://api.themoviedb.org/3/trending/tv/week?api_key=${this.apiKey}`)
      .then(res => {
        this.TrendingTv = res.data.results;
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

@media only screen and (max-width: 576px) {
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
