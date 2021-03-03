<template>
  <div class="back mt-1">
    <div class="container mt-3 mb-3">
      <div class="row justify-content-center">
        <div class="col-md-4">
          <img class="poster" :src="`https://image.tmdb.org/t/p/w500/${data.poster_path}`">
        </div>

        <div v-if="this.type === 'movie'">
          <div class="col-md-6">
            <h1 class="bold"> {{ data.title }} </h1>
            <div class="info">
              Release Date: <span class="bold">{{ data.release_date }}</span><br>
              Genre: <span class="bold">{{ setNames(data.genres) }}-</span> <br>
              Run Time: <span class="bold">{{ data.runtime }}m</span> <br>
            </div>
            <br>
            <div class="overview">
              <p>{{ data.overview }}</p>
            </div>
            <hr>
            <div class="studio">
              Production Studio:
              <p class="bold">
                {{ setNames(data.production_companies) }}-
              </p>
              Stars:
              <p class="bold">
                {{ setNames(cast) }}-
              </p>
            </div>
            <hr>
          </div>
        </div>

        
        <div v-else>
          <div class="col-md-6" v-if="this.type === 'tv'">
            <h1 class="bold"> {{ data.name }} </h1>
            <div class="info">
              Last Release Date: <span class="bold">{{ data.last_air_date }}</span><br>
              Genre: <span class="bold">{{ setNames(data.genres) }}-</span> <br>
              Episode Run Time: <span class="bold">{{ setRunTime(data.episode_run_time) }}- </span> <br>
              Last Season: <span class="bold">Season {{ data.number_of_seasons }}</span><br>
            </div>
            <br>
            <div class="overview">
              <p>{{ data.overview }}</p>
            </div>
            <hr>
            <div class="cast">
              Creator: <span class="bold"> {{ setNames(data.created_by) }}- </span> <br>
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
      id: '',
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      data: [],
      cast: []
    }
  },
  beforeMount() {
    this.type = this.$route.query.type
    this.id = this.$route.query.id
    this.getData()
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
    getData() {
      // this.movieId = this.$route.params.id
      axios.get(`https://api.themoviedb.org/3/${this.type}/${this.id}?api_key=${this.apiKey}`)
      .then(res => {
        this.data = res.data
      })
      .catch(err => {
        console.log(err);
      })
    },
    getCast() {
      axios.get(`https://api.themoviedb.org/3/${this.type}/${this.id}/credits?api_key=${this.apiKey}`)
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

@media only screen and (max-width: 768px) {
  .poster {
    width: 50%;
    margin-bottom: 10px;
  }
}

</style>