<template>
  <div class="color-row">
    <label :for="inputId" class="color-row__label">
      {{ this.colorModel }}
    </label>
    <input type=text readonly="readonly" class="color-row__input"
           :id="inputId" v-model="color" />
    <button class="button color-row__button" :id="buttonId"
            :data-clipboard-target="clipboardTarget"
            :class="{ invisible: !color }">
      <span class="fa fa-clipboard"></span>
    </button>
    <span class="color-row__copy-msg"
          :class="{ 'color-row__copy-msg--show': showCopyMsg }">
      Copied
    </span>
  </div>
</template>

<script>
import bus from './Bus'
import Clipboard from 'clipboard'

export default {
  name: 'ColorOut',
  data  () {
    return {
      color: '',
      showCopyMsg: false,
    }
  },
  props: {
    colorModel: {
      type: String,
      required: true,
      validator: (value) => /(Hex|RGB|RGBA|HSL|HSLA)/g.test(value),
    },
  },
  computed: {
    colorModelIdentifier () {
      return this.colorModel.toLowerCase()
    },
    inputId () {
      return `input-${this.colorModelIdentifier}`
    },
    buttonId () {
      return `button-${this.colorModelIdentifier}`
    },
    clipboardTarget () {
      return `#${this.inputId}`
    },
    event () {
      return `color-out:${this.colorModelIdentifier}`
    },
  },
  mounted () {
    bus.$on(this.event, colorObj => { this.color = colorObj })

    /* eslint-disable no-new */
    new Clipboard(`#${this.buttonId}`).on('success', () => {
      this.showCopyMsg = true
      setTimeout(() => { this.showCopyMsg = false }, 1000)
    })
  },
}
</script>

<style>

</style>
