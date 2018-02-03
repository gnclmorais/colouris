<template>
  <div></div>
</template>

<script>
import Vue from 'vue'
import colorConvert from 'color-convert'

const hexToString = (color) => {
  return color.startsWith('#') ? color : `#${color}`
}
const rgbToString = (r, g, b) => `rgb(${r}, ${g}, ${b})`
const rgbaToString = (r, g, b, a) => {
  return `rgba(${r}, ${g}, ${b}, ${Number(a).toFixed(1)})`
}
const hslToString = (h, s, l) => `hsl(${h}, ${s}%, ${l}%)`
const hslaToString = (h, s, l, a) => {
  return `hsla(${h}, ${s}%, ${l}%, ${Number(a).toFixed(1)})`
}

export default new Vue({
  data () {
    return {
      colorIn: '',
      colorOut: '',
    }
  },
  created () {
    this.$on('colorIn', newColor => { this.colorIn = newColor })
  },
  watch: {
    colorIn () {
      this.colorOut = this.convert(this.colorIn)
    },
    colorOut () {
      this.$emit('color-out:hex', this.colorOut.hex)
      this.$emit('color-out:rgb', this.colorOut.rgb)
      this.$emit('color-out:rgba', this.colorOut.rgba)
      this.$emit('color-out:hsl', this.colorOut.hsl)
      this.$emit('color-out:hsla', this.colorOut.hsla)

      if (this.colorOut.empty || this.colorOut.error) return
      this.$emit('color:validity', true)
    },
  },
  methods: {
    convert (rawColor) {
      // Empty input
      if (rawColor.length < 1) {
        this.$emit('color:validity', false)

        return { empty: true, }
      }

      if (this.isInvalid(rawColor)) {
        this.$emit('color:validity', false)

        return {
          error: 'Invalid color',
          value: rawColor,
        }
      }

      const color = this.clean(rawColor)

      let from
      let result = {}

      let a, b, c, alpha

      let conversionFn

      if (this.isHex(color)) {
        a = color

        from = 'hex'
        result.type = 'Hex'

        // Work around to translate color into its own type
        conversionFn = colorConvert[from]
        conversionFn.hex = () => color
      } else {
        // TODO: Deconstruct
        // const a, b, c, alpha = ...this.componentize(color)
        const components = this.componentize(color)

        if (this.isRGB(color)) {
          from = 'rgb'
          result.type = 'RGB'
          result[result.type.toLowerCase()] = color
        } else if (this.isRGBA(color)) {
          from = 'rgb'
          result.type = 'RGBA'
          result[result.type.toLowerCase()] = color
        } else if (this.isHSL(color)) {
          from = 'hsl'
          result.type = 'HSL'
          result[result.type.toLowerCase()] = color
        } else if (this.isHSLA(color)) {
          from = 'hsl'
          result.type = 'HSLA'
          result[result.type.toLowerCase()] = color
        } else {
          return {
            error: 'Invalid color',
            value: color,
          }
        }

        a = components[0]
        b = components[1]
        c = components[2]
        alpha = components[3]

        // Work around to translate color into its own type
        conversionFn = colorConvert[from]
        conversionFn[from] = () => components
      }

      // If there is any
      result.alpha = alpha || 1.0

      result.hex = hexToString(conversionFn.hex(a, b, c))
      result.rgb = rgbToString(...conversionFn.rgb(a, b, c))
      result.rgba = rgbaToString(...conversionFn.rgb(a, b, c), result.alpha)
      result.hsl = hslToString(...conversionFn.hsl(a, b, c))
      result.hsla = hslaToString(...conversionFn.hsl(a, b, c), result.alpha)

      return result
    },
    isInvalid (color) {
      // Leverage the browser parsing to see if the color is valid
      const div = document.createElement('div')
      div.style['background-color'] = color

      return !div.style['background-color']
    },
    clean (color) {
      // Basic clean, without dependencies on browser
      return color.trim().split(' ').join('').replace(/%/g, '')
    },
    isHex (color) {
      return color.match(/^#?([A-Fa-f0-9]){3}(([A-Fa-f0-9]){3})?$/gi)
    },
    isRGB (color) {
      // TODO: Fix 255 detection
      return color.match(/^(rgb\()(\d{1}|\d{2}|1\d{2}|2[0-5]{2}),(\d{1}|\d{2}|1\d{2}|2[0-5]{2}),(\d{1}|\d{2}|1\d{2}|2[0-5]{2})\)$/gi)
    },
    isRGBA (color) {
      // TODO: Fix 255 detection
      return color.match(/^(rgba\()(\d{1}|\d{2}|1\d{2}|2[0-5]{2}),(\d{1}|\d{2}|1\d{2}|2[0-5]{2}),(\d{1}|\d{2}|1\d{2}|2[0-5]{2}),(0?\.\d|1\.0)\)$/gi)
    },
    isHSL (color) {
      return color.match(/^(hsl\()(\d|\d{2}|[1-2]\d{2}|3[0-5]\d|360),((\d|\d{2}|100)),((\d|\d{2}|100))\)$/gi)
    },
    isHSLA (color) {
      return color.match(/^(hsla\()(\d|\d{2}|[1-2]\d{2}|3[0-5]\d|360),((\d|\d{2}|100)),((\d|\d{2}|100)),(0?\.\d|1\.0)\)$/gi)
    },
    componentize (color) {
      const start = color.indexOf('(') + 1
      const end = color.indexOf(')')
      return color.substring(start, end).split(',')
    },
  },
})
</script>

<style scoped>

</style>
