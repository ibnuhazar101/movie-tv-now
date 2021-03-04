<template>
  <div class="trending mt-3">
    <h3 class="text-center white">
      Trending This Week
    </h3>
    <hr class="bg-light">
    <div class="trend-mov" v-if="dataTrendingMovie.length > 0">
      <div class="trend mb-2" v-for="(movie, idMovieTrend) in dataTrendingMovie.slice(0, 4)" :key="idMovieTrend" @click="openDetail(movie)">
        <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`">
        <div class="trend-title">
          {{ movie.title }}
        </div>
        <div class="layer"></div>
      </div>
    </div>
    <div class="trend-mov" v-if="dataTrendingTv.length > 0">
      <div class="trend mb-2" v-for="(tv, idTvTrend) in dataTrendingTv.slice(0, 4)" :key="idTvTrend" @click="openDetailTv(tv)">
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
export default {
  props: {
    dataTrendingMovie: {
      type: Array,
      default: () => []
    },
    dataTrendingTv: {
      type: Array,
      default: () => []
    }
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
    openDetail(movie) {
      this.$router.push(`/detail?type=movie&typeId=${movie.id}`)
    },
    openDetailTv(tv) {
      this.$router.push(`/detail?type=tv&typeId=${tv.id}`)
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

@media only screen and (max-width: 576px) {
  .trend-mov {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
  
  .trend {
    width: 45%;
  }

  .trend-title {
    font-size: 16px;
  }

  .layer {
    height: 50%;
  }
}

</style>