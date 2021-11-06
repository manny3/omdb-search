<template>
  <div>
    <transition name="fade">
      <div
        v-if="isOpenModal"
        class="modal is-active"
      >
        <div class="modal-background" @click="isOpenModal = false"></div>
        <div class="modal-card">
          <loading :active.sync="isModalLoading" :is-full-page="false"></loading>
          <header class="modal-card-head">
            <p class="modal-card-title">{{ movieCount.title }}</p>
            <button @click="isOpenModal = false" class="delete" aria-label="close"></button>
          </header>
          <section class="modal-card-body">
            <div class="columns">
              <div class="column is-3">
                <figure class="image is-fullwidth">
                  <img :src="movieCount.poster" class="movie-image">
                </figure>
              </div>
              <div class="column is-9">
                <ul class="movie-content-list">
                  <li><strong>Actors:</strong> {{ movieCount.actors }}</li>
                  <li><strong>Genre:</strong> {{ movieCount.genre }}</li>
                  <li><strong>Plot:</strong><br>{{ movieCount.plot }}</li>
                </ul>
              </div>
            </div>
          </section>
          <footer class="modal-card-foot is-justify-content-flex-end">
            <button @click="isOpenModal = false" class="button is-dark">Cancel</button>
            <button @click="isOpenModal = false" class="button is-info">OK</button>
          </footer>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Loading from 'vue-loading-overlay'

export default {
  name: 'Modal',
  components: {
    Loading
  },
  props: {
    movieCount: {
      type: Object,
      default: () => {
        return {
          title: '',
          genre: '',
          actors: '',
          plot: '',
          poster: ''
        }
      }
    },
    isModalLoading: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      isOpenModal: false
    }
  },
  methods: {
    getDetail (imdbId) {
      this.$emit('get-movie-content', imdbId)
    },
    openModal () {
      this.isOpenModal = true
    }
  }
}
</script>

<style lang="scss" scoped>
  .modal-card {
    max-width: calc(100vh - 40px);
    .modal-card-title {
      text-align: left;
      font-weight: bold;
    }
    .modal-card-body {
      padding-top: 40px;
      padding-bottom: 40px;
    }
    .movie-content-list {
      list-style: circle;
      text-align: left;
      padding-left: 16px;
    }
    @media screen and (max-width: 768px) {
      .movie-image {
        width: 60%;
        margin: 0 auto;
      }
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: all .5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
    transform: translateＹ(10px);
  }
</style>
