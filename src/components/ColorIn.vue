<template>
  <div>
    <div class="color-row color-in">
      <label for="color-in" class="color-in__label">
        Write/paste your color here
      </label>
      <input id="color-in" type=text class="color-in__input" autofocus
             placeholder="Place a colour here"
             v-model="color" @keyup="colorIn"
             :style="{ 'background-color': backgroundColor,
                       color: foregroundColor }" />
      <span class="fa fa-check color-in__valid"
            :class="{ invisible: !valid }"
            :style="{ color: foregroundColor }"></span>
    </div>
  </div>
</template>

<script>
import invert from 'invert-color'

import Bus from './Bus'
import ColorRow from '../mixins/ColorRow'

export default {
  name: 'ColorIn',
  extends: ColorRow,
  data  () {
    return {
      color: '',
      colorInvert: '',
      valid: false,
    }
  },
  computed: {
    isEmpty () {
      return this.color.length === 0
    },
    backgroundColor () {
      return this.isEmpty ? 'inherit' : this.valid && this.color
    },
    foregroundColor () {
      return this.isEmpty ? 'inherit' : this.valid && this.colorInvert
    },
  },
  methods: {
    colorIn () {
      Bus.$emit('colorIn', this.color)
      Bus.$on('color:validity', this.updateColorValidity)
      Bus.$on('color-out:hex', this.updateColorText)
    },
    updateColorValidity (validity) {
      this.valid = validity
    },
    updateColorText () {
      try {
        this.colorInvert = invert(this.color, true)
      } catch (error) {
        // Send the error to oblivion.
        // That's OK, the library just doesn't know how to deal with non-hex.
      }
    },
  },
}
</script>

<style scoped>
.color-in {
  position: relative;
}

.color-in__label {
  display: none;
}

.color-in__input {
  flex: 1 0;
  margin: 0 50px 0 60px;
  color: whitesmoke;
  transition: color, background-color 500ms ease-out;
}

.color-in__input::-webkit-input-placeholder { color: lightgray; }
.color-in__input::-moz-placeholder          { color: lightgray; }
.color-in__input:-ms-input-placeholder      { color: lightgray; }
.color-in__input:-moz-placeholder           { color: lightgray; }

.color-in__valid {
  position: absolute;
  top: 50%;
  right: 64px;
  margin-top: -.5em;
}
</style>
