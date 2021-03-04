<template>
  <div class="back mt-1">
    <div class="container mt-3 mb-3">

      <div v-if="type === 'movie'">
        <div class="row justify-content-center">
          <div class="col-md-4">
            <img class="poster" :src="`http://image.tmdb.org/t/p/w500/${dataDetail.poster_path}`">
          </div>
          <div class="col-md-6">
            <h1 class="bold"> {{ dataDetail.title }} </h1>
            <div class="info">
              Release Date: <span class="bold">{{ dataDetail.release_date }}</span><br>
              Genre: <span class="bold">{{ setNames(dataDetail.genres) }}-</span> <br>
              Run Time: <span class="bold">{{ dataDetail.runtime }}m</span> <br>
            </div>
            <br>
            <div class="overview">
              <p>{{ dataDetail.overview }}</p>
            </div>
            <hr>
            <div class="studio">
              Production Studio:
              <p class="bold">
                {{ setNames(dataDetail.production_companies) }}-
              </p>
              Stars:
              <p class="bold">
                {{ setNames(cast) }}-
              </p>
            </div>
            <hr>
          </div>
        </div>
      </div>
     
      <div v-else>
        <div class="row justify-content-center">
          <div class="col-md-4">
            <img class="poster" :src="`http://image.tmdb.org/t/p/w500/${dataDetail.poster_path}`">
          </div>
          <div class="col-md-6">
            <h1 class="bold"> {{ dataDetail.name }} </h1>
            <div class="info">
              Last Release Date: <span class="bold">{{ dataDetail.last_air_date }}</span><br>
              Genre: <span class="bold">{{ setNames(dataDetail.genres) }}-</span> <br>
              Episode Run Time: <span class="bold">{{ setTime(dataDetail.episode_run_time) }}- </span> <br>
              Last Season: <span class="bold">Season {{ dataDetail.number_of_seasons }}</span><br>
            </div>
            <br>
            <div class="overview">
              <p>{{ dataDetail.overview }}</p>
            </div>
            <hr>
            <div class="cast">
              Creator: <span class="bold"> {{ setNames(dataDetail.created_by) }}- </span> <br>
              Stars: <span class="bold"> {{ setNames(cast) }}- </span> 
            </div>
            <hr>
          </div>
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
      type: '',
      typeId: '',
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataDetail: [],
      cast: []
    }
  },
  beforeMount() {
    this.type = this.$route.query.type
    this.typeId = this.$route.query.typeId
    this.getDataDetail()
    this.getCast()
  },
  methods: {
    setNames(arrayNames) {
      let names = ''
      arrayNames && arrayNames.forEach(element => {
        names += ` ${element.name},`
      });
      return names
    },
    setTime(arrayNames) {
      let names = ''
      arrayNames && arrayNames.forEach(element => {
        names += ` ${element}m,`
      });
      return names
    },
    getDataDetail() {
      axios.get(`https://api.themoviedb.org/3/${this.type}/${this.typeId}?api_key=${this.apiKey}`)
      .then(res => {
        this.dataDetail = res.data
      })
      .catch(err => {
        console.log(err);
      })
    },
    getCast() {
      axios.get(`https://api.themoviedb.org/3/${this.type}/${this.typeId}/credits?api_key=${this.apiKey}`)
      .then(res => {
        this.cast = res.data.cast.slice(0, 10)
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
  padding: 10px 0;
}

.bold {
  font-weight: bold;
}

.poster {
  width: 100%;
  border-radius: 10px;
}

@media only screen and (max-width: 576px) {
  .poster {
    width: 50%;
  }
}

@media only screen and (max-width: 768px) {
  .container {
    font-size: 12px;
  }
}

</style>