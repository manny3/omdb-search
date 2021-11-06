<template>
  <div class="search-form">
    <div class="field">
      <label class="label">Movie Title</label>
      <div class="control">
        <input
          v-model="movieTitle"
          v-on:change="$emit('update:s', $event.target.value)"
          v-on:keydown.13="searchSubmit()"
          :class="[{ 'is-danger': errorPrompt}]"
          errorPrompt
          class="input"
          type="text"
          placeholder=""
        >
      </div>
      <p v-show="errorPrompt" class="help is-danger">Movie Title is required</p>
    </div>

    <div class="columns">
      <div class="column is-half">
        <div class="field">
          <label class="label">Type</label>
          <div class="control">
            <div class="select">
              <select
                v-model="movieType"
                v-on:change="$emit('update:type', $event.target.value)"
              >
                <option value="">Select Type</option>
                <option value="movie">movie</option>
                <option value="series">series</option>
                <option value="episode">episode</option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-half">
        <div class="field">
          <label class="label">Year</label>
          <div class="control">
            <div class="select">
              <select
                v-model="movieYear"
                v-on:change="$emit('update:y', $event.target.value)"
              >
                <option value="">Select Year</option>
                <option v-for="year in yearList" :key="year" :value="year">{{year}}</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="field">
      <div class="control">
        <button class="button is-info" @click="searchSubmit()">Search</button>
      </div>
    </div>
  </div>
</template>

<script>
import years from '@/assets/json/years.json'

export default {
  name: 'SearchForm',
  props: {
    s: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: ''
    },
    y: {
      type: String,
      default: ''
    },
    page: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      errorPrompt: false,
      yearList: [],
      movieTitle: this.s,
      movieType: this.type,
      movieYear: this.y
    }
  },
  methods: {
    searchSubmit () {
      if (this.checkEmptyText()) return

      if (this.page !== 1) {
        this.$emit('resetPage')
      }

      this.$emit('getSearchMovieList')
    },
    checkEmptyText () {
      if (this.movieTitle.trim() === '') {
        this.errorPrompt = true
        return true
      } else {
        this.errorPrompt = false
        return false
      }
    }
  },
  created () {
    this.yearList = years
  }
}
</script>

<style lang="scss" scoped>
  .search-form {
    max-width: 400px;
    margin: 0 auto;
  }
</style>
