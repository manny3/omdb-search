<template>
  <div id="app">
    <div class="main-content">
      <div class="container">
        <section class="mb-6">
          <SearchForm
            :s.sync="query.s"
            :type.sync="query.type"
            :y.sync="query.y"
            :page.sync="query.page"
            @resetPage="resetPage"
            @getSearchMovieList="getSearchMovieList"
          />
        </section>

        <section class="mb-6">
          <MovieTable
            :movie-list="movieList"
            :table-prompt="tablePrompt"
            :is-loading="isLoading"
            @get-movie-content="getMovieContent"
          />
        </section>

        <div class="mb-6">
          <Pagination
            :current="pagination.current"
            :total="pagination.total"
            :itemsPerPage="pagination.itemsPerPage"
            :step="pagination.step"
            :onChange="onChange"
          />
        </div>

        <div>
          <Modal
            ref="modal"
            :movieCount="movieCount"
            :isModalLoading="isModalLoading"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import 'vue-loading-overlay/dist/vue-loading.css'

import years from './assets/json/years.json'
import SearchForm from './components/SearchForm'
import MovieTable from './components/MovieTable'
import Pagination from './components/Pagination'
import Modal from './components/Modal'

const apiUrl = process.env.VUE_APP_API_URL

export default {
  name: 'App',
  components: {
    SearchForm,
    MovieTable,
    Pagination,
    Modal
  },
  data () {
    return {
      movieList: [],
      tablePrompt: 'No data',
      isLoading: false,

      pagination: {
        current: 1,
        total: 0,
        itemsPerPage: 10,
        step: 1
      },
      query: {
        apikey: process.env.VUE_APP_API_KEY,
        s: '',
        type: '',
        y: '',
        page: 1
      },

      openModal: false,
      isModalLoading: false,
      movieCount: {
        title: '',
        genre: '',
        actors: '',
        plot: '',
        poster: ''
      }
    }
  },
  methods: {
    getSearchMovieList () {
      this.isLoading = true
      const params = this.query
      axios.get(apiUrl, { params })
        .then((res) => {
          if (res.data.Response === 'False') {
            this.movieList = []
            this.pagination.total = 0
            this.tablePrompt = res.data.Error
            setTimeout(() => {
              this.isLoading = false
            }, 500)
            return
          }
          const total = Number(res.data.totalResults)
          this.pagination.total = total
          this.movieList = res.data.Search
          setTimeout(() => {
            this.isLoading = false
          }, 500)
        })
    },
    getMovieContent (id) {
      this.isModalLoading = true
      this.$refs.modal.openModal()
      const params = {
        apikey: process.env.VUE_APP_API_KEY,
        i: id
      }
      axios.get(apiUrl, { params })
        .then((res) => {
          const { Title, Genre, Actors, Plot, Poster } = res.data
          this.movieCount.title = Title
          this.movieCount.genre = Genre
          this.movieCount.actors = Actors
          this.movieCount.plot = Plot
          this.movieCount.poster = Poster
          setTimeout(() => {
            this.isModalLoading = false
          }, 500)
        })
    },
    resetPage () {
      this.pagination.current = 1
      this.query.page = 1
    },
    onChange (page) {
      this.query.page = page
      this.pagination.current = page
      this.getSearchMovieList()
    }
  },
  created () {
    this.yearList = years
  }
}
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .main-content {
    padding: 0 1.5rem;
  }
</style>
