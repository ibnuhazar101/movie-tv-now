<template>
  <div>
    <div class="row justify-content-center">
      <div class="col-md-3" id="trending">
        <Trending />
      </div>
      <div class="col-md-6" id="movie">
        <Movie />
      </div>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style>
body {
  background-color: rgb(26, 47, 66);
}

@media only screen and (max-width: 768px) {
  .row {
    display: flex;
    flex-direction: column;
    padding: 10px;
  }
  #movie {
    order: 1;
    width: 100%;
  }
  #trending {
    order: 2;
    width: 100%;
  }
}

</style>
