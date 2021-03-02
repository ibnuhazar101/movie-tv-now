<template>
  <div> 
    <h1 class="tv-title mt-3 text-right">Now Streaming</h1>
    <div class="tvs">
      <div v-for="(tv, idTv) in dataTv" :key="idTv" class="tv-list" @click="openDetail(tv)">
        <img :src="`http://image.tmdb.org/t/p/w500${tv.poster_path}`">
        <div class="tv-title">
          {{ tv.name }} 
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    dataTv: {
      type: Array,
      default: []
    },
    page: Number
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
}

</style>