<template>
  <div id="app">
    <div v-if="session">
      <nprogress-container></nprogress-container>
      <navbar :show="true"></navbar>
      <sidebar :show="sidebar.opened && !sidebar.hidden"></sidebar>
      <app-main></app-main>
    </div>
    <div v-if="!session">
      <nprogress-container></nprogress-container>
      <login></login>
    </div>
  </div>
</template>

<script>
import NprogressContainer from 'vue-nprogress/src/NprogressContainer'
import { Navbar, Sidebar, Login, AppMain } from './components/layout/'
import { mapGetters, mapActions } from 'vuex'
import auth from './auth'
import moment from 'moment'

export default {
  components: {
    Navbar,
    Sidebar,
    Login,
    AppMain,
    NprogressContainer
  },

  beforeMount () {
    const { body } = document
    const WIDTH = 768
    const RATIO = 3

    const handler = () => {
      if (!document.hidden) {
        let rect = body.getBoundingClientRect()
        let isMobile = rect.width - RATIO < WIDTH
        this.toggleDevice(isMobile ? 'mobile' : 'other')
        this.toggleSidebar({
          opened: !isMobile
        })
      }
    }

    document.addEventListener('visibilitychange', handler)
    window.addEventListener('DOMContentLoaded', handler)
    window.addEventListener('resize', handler)
  },

  mounted () {
    this.checkAuth()
  },

  watch: {
    '$route': 'checkAuth'
  },

  computed: mapGetters({
    sidebar: 'sidebar',
    session: 'session'
  }),

  methods: {

    checkAuth () {
      let session = auth.getSession()
      if (session) {
        // check if jwt has been expired
        if (moment().isAfter(moment.unix(session['jwtexpiry']))) {
          auth.logout(this)
        } else {
          this.$store.commit('setSession', session)
        }
      }
    },

    ...mapActions([
      'toggleDevice',
      'toggleSidebar'
    ])
  }
}
</script>

<style lang="scss">
@import '~animate.css';
.animated {
  animation-duration: .377s;
}

@import '~bulma';
@import '~wysiwyg.css/wysiwyg.sass';

$fa-font-path: '~font-awesome/fonts/';
@import '~font-awesome/scss/font-awesome';

html {
  background-color: rgb(42, 38, 53);
}

@font-face {
  font-family: 'Lobster';
  src: url('assets/fonts/Lobster-Regular.ttf');
}

.nprogress-container {
  position: fixed !important;
  width: 100%;
  height: 50px;
  z-index: 2048;
  pointer-events: none;

  #nprogress {
    $color: #48e79a;

    .bar {
      background: $color;
    }
    .peg {
      box-shadow: 0 0 10px $color, 0 0 5px $color;
    }

    .spinner-icon {
      border-top-color: $color;
      border-left-color: $color;
    }
  }
}

.is-primary {
  background-color: #4da2fc !important;
  font-weight: bold;
  border-color: transparent;
  color: whitesmoke;
}

.is-primary:hover, .button:active, .button:focus {
  color: whitesmoke;
  border-color: transparent;
}

.is-green-button {
  background-color: #4CAF50 !important;
  font-weight: bold;
  border-color: transparent;
  color: whitesmoke;
}

.is-green-button:hover, .button:active, .button:focus {
  color: whitesmoke;
  border-color: transparent;
}

.is-disabled {
  opacity: .5;
  pointer-events: none;
}

.content-article {
  color: whitesmoke;
  background-color: #3f3d49;
}

.label {
  color: whitesmoke;
  font-weight: normal;
}

.input-bar {
  background-color: #19191b;
  color: white;
  border-color: #2a2735;
}

.input-bar::-webkit-input-placeholder {
    color: #8c91a0;
    text-shadow: none;
    -webkit-text-fill-color: initial;
}

.input-bar::-moz-placeholder {
    color: #8c91a0;
    text-shadow: none;
    opacity: 1;
}

.title-text {
  border-bottom: 1px whitesmoke solid;
  padding-bottom: 8px;
  text-align: center;
  color: #ff651d !important;
}

.modal-z-index {
  z-index: 1025;
}

@media screen and (min-width: 768px) {
  .modal-content {
    width: 480px; /* either % (e.g. 60%) or px (400px) */
  }
}

.collapse-item {
  background-color: black;

  .card-header {
    background-color: #3f3d49;
  }
}

.card-header-title {
  color: whitesmoke;
}

.select select {
  background-color: #19191b;
  color: white;
  border-color: #2a2735;
}

.is-blue {
  color: #4da2fc !important;
}

/* Table styles */
.table {
  background-color: #19191b !important;
  color: white !important;
  border-color: whitesmoke;
  width: 100%;
}

.progress-bar-middle {
  position: relative;
  -webkit-transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
  top: 50%;
}

.progress-bar-height {
  height: 50px;
}

.table thead {
  border: 0 !important;
  color: #8c91a0 !important;
  text-align: center !important;
}

.table td  {
  border-top: solid black 1px !important;
  border-bottom: solid black 1px !important;
  color: #8c91a0 !important;
  text-align: center !important;
}

.table-grid  thead th {
  color: #4da2fc;
  text-align: center !important;
}

.table-own-bordered {
  border-collapse: separate !important;
  border: solid black 1px;
  border-radius: 6px;
}

.responsive {
  overflow-x: auto !important;
}

.vgt-wrap__footer {
  border: solid black 1px !important;
  border-radius: 6px;
  margin-top: 10px !important;
  color: whitesmoke !important;
  background: #19191b !important;
}

.vgt-global-search {
  border: solid black 1px !important;
  border-radius: 6px;
  margin-bottom: 10px !important;
  color: whitesmoke !important;
  background: #19191b !important;
}

.vgt-input {
  background-color: #19191b !important;
  color: white !important;
  border-color: #2a2735 !important;
}

.vgt-input::-webkit-input-placeholder {
  color: #8c91a0 !important;
  text-shadow: none !important;
  -webkit-text-fill-color: initial !important;
}

.vgt-input::-moz-placeholder {
  color: #8c91a0 !important;
  text-shadow: none !important;
  opacity: 1 !important;
}

.empty-table-text {
  color: #8c91a0;
  text-align: center;
}

</style>
