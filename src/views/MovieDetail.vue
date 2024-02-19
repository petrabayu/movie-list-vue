<template>
  <div class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p class="year">{{ movie.Year }}</p>
    <img class="featured-img" :src="movie.Poster" :alt="movie.Title" />
    <p>{{ movie.Plot }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue'
import { useRoute } from 'vue-router'
import env from '@/env'

export default {
  setup() {
    const movie = ref({})
    const route = useRoute()

    onBeforeMount(() => {
      fetch(`http://www.omdbapi.com/?apikey=${env.API_KEY}&i=${route.params.id}&plot=full`)
        .then((response) => response.json())
        .then((data) => {
          movie.value = data
        })
    })

    return {
      movie,
      route
    }
  }
}
</script>

<style lang="scss">
.movie-detail {
  padding: 16px;
  text-align: center;
  max-width: 600px;
  margin: 0 auto;

  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .year {
    text-align: center;
  }

  .featured-img {
    margin: 32px 0px;
  }

  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;
    text-align: left;
  }
}
</style>
