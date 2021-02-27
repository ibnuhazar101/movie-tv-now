<template>
  <div class=""> 
    <h1 class="tv-title mt-3 text-right">Now Streaming</h1>
    <div class="tvs">
      <div v-for="(tv, idTv) in dataTv" :key="idTv" class="tv-list" @click="openDetail(tv)">
        <img :src="`http://image.tmdb.org/t/p/w500${tv.poster_path}`">
        <div class="tv-title">
          {{ tv.name }} 
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
      dataTv: [],
      page: 1
    }
  },
  mounted() {
    this.getDataTv()
  },
  methods: {
    getDataTv() {
      axios.get(`https://api.themoviedb.org/3/tv/on_the_air?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        console.log(res.data.page);
        this.dataTv = res.data.results;
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
      axios.get(`https://api.themoviedb.org/3/tv/on_the_air?api_key=${this.apiKey}&page=${this.page}`)
      .then(res => {
        this.dataTv = [...this.dataTv, ...res.data.results];
        return this.dataTv;
      })
    },
    scrollBehavior() {
        window.scrollTo(0,0);
    },
    openDetail(tv) {
      this.$router.push(`/tv/${tv.id}`)
    }
  }
}
</script>

<style>

.tvs {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0 auto;
}

.tv-list {
  width: 8.5rem;
  margin-bottom: 1rem;
  cursor: pointer;
}

.tv-list img {
  width: 8.5rem;
}

.tv-title {
  color: white;
  /* font-weight: bold; */
}

</style>