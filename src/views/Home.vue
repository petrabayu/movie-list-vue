<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt8579674">
        <img
          src="https://m.media-amazon.com/images/M/MV5BOTdmNTFjNDEtNzg0My00ZjkxLTg1ZDAtZTdkMDc2ZmFiNWQ1XkEyXkFqcGdeQXVyNTAzNzgwNTg@._V1_SX300.jpg"
          alt="Naruto Poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>1917</h3>
          <p>
            April 6th, 1917. As an infantry battalion assembles to wage war deep in enemy territory,
            two soldiers are assigned to race against time and deliver a message that will stop
            1,600 men from walking straight into a deadly trap.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for ?" v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" :alt="movie.Title" />
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import env from '@/env'

export default {
  setup() {
    const search = ref('')
    const movies = ref([])

    const SearchMovies = () => {
      if (search.value != '') {
        fetch(`http://www.omdbapi.com/?apikey=${env.API_KEY}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search
            search.value = ''
          })
      }
    }

    return {
      search,
      movies,
      SearchMovies
    }
  }
}
</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;
    max-width: 600px;
    margin: 0 auto;

    .featured-img {
      display: block;
      width: 100%;
      height: 400px;
      object-fit: cover;
      position: relative;
      /* object-position: bottom; */
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;
    max-width: 600px;
    margin: 0 auto;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px 0;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type='text'] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 16px;
        transition: 0.4s;

        &::placeholder {
          color: #bbbbbb;
        }

        &:focus {
          border: 1px solid #fff;
        }
      }

      &[type='submit'] {
        width: 100%;
        background-color: #42b883;
        padding: 16px 0;
        border-radius: 8px;
        color: #fff;
        font-size: 18px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px auto;
    /* width: 100%; */
    max-width: 1200px;

    .movie {
      padding: 16px 8px;
      max-width: 200px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 300px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom: 16px;
            left: 0;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaa;
            font-size: 14px;
            margin-bottom: 8px;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
