<template>
  <div class="back mt-1">
    <div class="container mt-3 mb-3">
      <div class="row justify-content-center">
        <div class="col-4">
          <img class="poster" :src="`https://image.tmdb.org/t/p/w500/${dataTv.poster_path}`">
        </div>
        <div class="col-6">
          <h1 class="bold"> {{ dataTv.name }} </h1>
          Last Release Date: <span class="bold">{{ dataTv.last_air_date }}</span><br>
          Genre: 
          <div v-for="(genre, idGenreTv) in dataTv.genres" :key="idGenreTv" class="d-inline bold"> {{ genre.name }},</div>-<br>
          Episode Run Time:
          <div v-for="(time, idTimeTv) in dataTv.episode_run_time" :key="idTimeTv" class="d-inline bold"> {{ time }}m,</div>-<br>
          Last Season: <span class="bold">Season {{ dataTv.number_of_seasons }}</span><br>
          <br>
          <p>
            {{ dataTv.overview }}
          </p>
          <hr>
          Creator: 
          <div v-for="(creator, idCreatorTv) in dataTv.created_by" :key="idCreatorTv" class="d-inline bold"> {{ creator.name }},</div>-<br>
          Stars:
          <div v-for="(stars, idStarsTv) in tvCast.slice(0, 10)" :key="idStarsTv" class="d-inline bold"> {{ stars.name }},</div>-<br>
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
      tvId: '',
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataTv: [],
      dataTvCredit: [],
      tvCast: []
    }
  },
  mounted() {
    this.getDataTvs()
  },
  methods: {
    getDataTvs() {
      this.tvId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/tv/${this.tvId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataTv = res.data
        console.log(res);
      })
      .catch(err => {
        console.log(err);
      }),

      axios.get(`https://api.themoviedb.org/3/tv/${this.tvId}/credits?api_key=${this.apiKey}`)
      .then(res => {
        this.dataTvCredit = res.data
        this.tvCast = res.data.cast
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
  height: 100%;
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