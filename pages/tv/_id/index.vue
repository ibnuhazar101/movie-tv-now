<template>
  <div class="back mt-1">
    <div class="container mt-3 mb-3">
      <div class="row justify-content-center">
        <div class="col-md-4">
          <img class="poster" :src="`https://image.tmdb.org/t/p/w500/${dataTv.poster_path}`">
        </div>
        <div class="col-md-6">
          <h1 class="bold"> {{ dataTv.name }} </h1>
          <div class="info">
            Last Release Date: <span class="bold">{{ dataTv.last_air_date }}</span><br>
            Genre: <span class="bold">{{ setNames(dataTv.genres) }}-</span> <br>
            Episode Run Time: <span class="bold">{{ setRunTime(dataTv.episode_run_time) }}- </span> <br>
            Last Season: <span class="bold">Season {{ dataTv.number_of_seasons }}</span><br>
          </div>
          <br>
          <div class="overview">
            <p>{{ dataTv.overview }}</p>
          </div>
          <hr>
          <div class="cast">
            Creator: <span class="bold"> {{ setNames(dataTv.created_by) }}- </span> <br>
            Stars: <span class="bold"> {{ setNames(tvCast) }}- </span> 
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
      tvId: '',
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataTv: [],
      tvCast: []
    }
  },
  mounted() {
    this.getDataTvs()
    this.getDataCast()
  },
  methods: {
    setNames(arrayNames) {
      let names = ''
      arrayNames && arrayNames.forEach(element => {
        names += ` ${element.name},`
      });
      return names
    },
    setRunTime(arrayNames) {
      let names = ''
      arrayNames && arrayNames.forEach(element => {
        names += ` ${element}m,`
      });
      return names
    },
    getDataTvs() {
      this.tvId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/tv/${this.tvId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataTv = res.data
      })
      .catch(err => {
        console.log(err);
      })
    },
    getDataCast() {
      axios.get(`https://api.themoviedb.org/3/tv/${this.tvId}/credits?api_key=${this.apiKey}`)
      .then(res => {
        this.tvCast = res.data.cast.slice(0, 10)
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

@media only screen and (max-width: 768px) {
  .poster {
    width: 50%;
    margin-bottom: 10px;
  }
}
</style>