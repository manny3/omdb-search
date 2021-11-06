<template>
  <div class="table-container">
    <table class="table is-bordered is-striped movie-table">
      <loading :active.sync="isLoading" :is-full-page="false"></loading>
      <thead>
        <tr>
          <th class="movie-table-title">名稱</th>
          <th>類型</th>
          <th class="movie-table-year">年份</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-show="movieList.length === 0"><td colspan="4" class="empty-data">{{ tablePrompt }}</td></tr>
        <tr v-for="movie in movieList" :key="movie.imdbID">
          <th>{{movie.Title}}</th>
          <td>{{movie.Type}}</td>
          <td>{{movie.Year}}</td>
          <td><button class="button is-info is-small" @click="getDetail(movie.imdbID)">詳細資料</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Loading from 'vue-loading-overlay'

export default {
  name: 'MovieTable',
  components: {
    Loading
  },
  props: {
    movieList: {
      type: Array,
      default: () => {
        return []
      }
    },
    tablePrompt: {
      type: String,
      default: ''
    },
    isLoading: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    getDetail (imdbId) {
      this.$emit('get-movie-content', imdbId)
    }
  }
}
</script>

<style lang="scss" scoped>
  .movie-table {
    position: relative;
    margin: 0 auto;
    .empty-data {
      font-weight: bold;
    }
    .movie-table-title {
      width: 500px;
    }

    .movie-table-year {
      width: 120px;
    }
  }
</style>
