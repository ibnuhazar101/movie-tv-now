<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <NuxtLink class=" mr-5 ml-5 brand" to="/">movieTvNow</NuxtLink>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <NuxtLink to="/" class="nav-link">Movies</NuxtLink>
            </li>
            <li class="nav-item">
              <NuxtLink to="/tvShows" class="nav-link">TV Shows</NuxtLink>
            </li>
          </ul>
          <form @submit.prevent="" class="d-flex ml-auto">
            <input class="form-control me-2" type="search" placeholder="Search movie/tv..." v-model="search">
            <button class="btn btn-outline-success" type="submit" @click="showResults()">Search</button>
          </form>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: ''
    }
  },
  methods: {
    showResults() {
      this.$router.push(`/search/${this.search}`) 
    }
  }
}
</script>

<style>
.brand {
  color: rgb(35, 64, 90);
  font-weight: bold;
  font-size: 18px;
}

.brand:hover {
  text-decoration: none;
  color: rgb(35, 64, 90);
}

@media only screen and (max-width: 768px) {
  .brand {
    margin-left: 0 !important;
  }
}

</style>