<template>
  <div class="trending mt-3">
    <h3 class="text-center white">
      Trending This Week
    </h3>
    <hr class="bg-light">
    <div class="trend-mov">
      <h4 class="white">Movie</h4>
      <div class="trend mb-2" v-for="(movie, idMovieTrend) in dataMovies.slice(0, 3)" :key="idMovieTrend" @click="openDetail(movie)">
        <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
        <div class="trend-title">
          {{ movie.title }}
        </div>
        <div class="layer"></div>
      </div>
    </div>
    <hr class="bg-light">
    <div class="trend-mov">
      <h4 class="white">TV</h4>
      <div class="trend mb-2" v-for="(tv, idTvTrend) in dataTv.slice(0, 3)" :key="idTvTrend" @click="openDetailTv(tv)">
        <img :src="`http://image.tmdb.org/t/p/w500${tv.poster_path}`">
        <div class="trend-title">
          {{ tv.name }}
        </div>
        <div class="layer"></div>
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
      dataTv: []
    }
  },
  mounted() {
    this.getDataMovies()
  },
  methods: {
    getDataMovies() {
      axios.get(`https://api.themoviedb.org/3/trending/movie/week?api_key=${this.apiKey}`)
      .then(res => {
        this.dataMovies = res.data.results;
      }),
      axios.get(`https://api.themoviedb.org/3/trending/tv/week?api_key=${this.apiKey}`)
      .then(res => {
        this.dataTv = res.data.results;
      })
    },
    changeDate(date) {
      var d = new Date(date);
      var n = d.getFullYear();
      return n;
    },
    openDetail(movie) {
      this.$router.push(`/movie/${movie.id}`)
    },
    openDetailTv(tv) {
      this.$router.push(`/tv/${tv.id}`)
    }
  }
}
</script>

<style>

.trend-mov {
  position: relative;
}

.trend {
  position: relative;
  cursor: pointer;
}

.trend-title {
  display: flex;
  position: absolute;
  bottom: 5px;
  left: 5px;
  font-size: 24px;
  color: white;
  z-index: 1;
}

.trend-mov img {
  position: relative;
  width: 100%;
  height: 130px;
  object-fit: cover;
  border-radius: 10px 10px 0 0;
  z-index: -1;
}

.layer {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 60px;
  background-color: black;
  opacity: 0.5;
}

.white {
  color: white;
}

</style>