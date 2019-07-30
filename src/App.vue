<template>
  <transition name="fade">
    <div class="app" v-show="loaded">
      <div class="app-content">
        <div class="app-content-wrapper">
          <h1 class="text-center">Colouris</h1>
          <color2color></color2color>
        </div>
      </div>
      <footer class="app-footer text-center">
        <a class="app-footer__link" href="https://twitter.com/gnclmorais">
          <span class="fa fa-twitter"></span>gnclmorais
        </a>
      </footer>
    </div>
  </transition>
</template>

<script>
import Color2Color from './components/Color2Color'

export default {
  name: 'App',
  components: {
    color2color: Color2Color,
  },
  data () {
    return {
      loaded: false,
    }
  },
  mounted () {
    this.loaded = true
  },
}
</script>

<style>
/**
 * General
 */

html,
body {
  height: 100%;
  margin: 0;
}

body {
  font-family: 'Roboto Slab', serif;
  /* Background from https://uigradients.com/#Mantle */
  background: #24C6DC;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #514A9D, #24C6DC);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #514A9D, #24C6DC); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  color: whitesmoke;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

.app-content {
  flex: 1;
  display: flex;
  justify-content: center;
}

.app-content-wrapper {
  flex-grow: 0;
  align-self: center;
}

.app-footer {
  margin-bottom: 10px;
}

.app-footer__link {
  color: whitesmoke;
}

.app-footer__link .fa {
  position: relative;
  bottom: -1px;
}

/**
 * Utilities
 */

.center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.text-center {
  text-align: center;
}

.text-right {
  text-align: right;
}

.invisible {
  visibility: hidden;
}

/**
 * Animations
 */

.fade-enter-active {
  transition: opacity 300ms ease-in-out;
}

.fade-enter-to {
  opacity: 1;
}

.fade-enter {
  opacity: 0;
}

/**
 * General overrides
 */

input[type='text'] {
  font-size: 1em;
}

input[type='text']:focus {
  border-color: white
}
</style>
