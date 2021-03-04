<template>
  <div>
    
    <div class="row justify-content-center">
      <div class="col-md-6" id="movie">
        <div> 
          <h1 class="mov-title mt-3 text-right">Search Results</h1>
          <div class="movies">
            <div v-for="(content, id) in dataContents" :key="id" class="movie-list" @click="openDetail(content)">
              <img :src="`http://image.tmdb.org/t/p/w500${content.poster_path}`">
              <div class="mov-title">
                <div v-if="content.media_type == 'movie'">
                  {{ content.title }} ({{ changeDate(content.release_date) }})
                </div>
                <div v-else-if="content.media_type == 'tv'">
                  {{ content.name }}
                </div>
                <div v-else>
                  NaN
                </div>
              </div>
            </div>
          </div>
          <div class="d-block btn btn-light mt-3 mb-3" @click="setPage()">
            More
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
      apiKey: '6de3c0f0176c22fabe34c6be66fa8cae',
      dataContents: [],
      page: 1,
      searchId: ''
    }
  },
  watch: {
    page() {
      this.getDataContents()
    },
    $route(to, from) {
      if(to !== from) {
        location.reload();
      }
    }
  },
  beforeMount() {
    this.searchId = this.$route.query.searchId
    this.getDataContents()
  },
  methods: {
    getDataContents() {
      axios.get(`https://api.themoviedb.org/3/search/multi?api_key=${this.apiKey}&query=${this.searchId}&page=${this.page}`)
      .then(res => {
        this.dataContents = [...this.dataContents, ...res.data.results]
        return this.dataContents
      })
      .catch(err => {
        console.log(err);
      })
    },
    changeDate(date) {
      var d = new Date(date);
      var n = d.getFullYear();
      return n;
    },
    setPage() {
      this.page += 1;
    },
    openDetail(content) {
        this.$router.push(`/detail?type=${content.media_type}&typeId=${content.id}`)
    }
  }
}
</script>

<style>

.movies {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0 auto;
}

.movie-list {
  width: 8.5rem;
  margin-bottom: 1rem;
  cursor: pointer;
}

.movie-list img {
  width: 100%;
}

.mov-title {
  color: white;
}

</style>