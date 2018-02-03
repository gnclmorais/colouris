<template>
  <div>
    <color-in></color-in>

    <div class="text-center">
      <span class="fa fa-chevron-down"></span>
    </div>

    <color-out color-model="Hex" ></color-out>
    <color-out color-model="RGB" ></color-out>
    <color-out color-model="RGBA"></color-out>
    <color-out color-model="HSL" ></color-out>
    <color-out color-model="HSLA"></color-out>
  </div>
</template>

<script>
import ColorIn from './ColorIn'
import ColorOut from './ColorOut'

export default {
  name: 'Color2Color',
  components: {
    'color-in': ColorIn,
    'color-out': ColorOut,
  },
}
</script>

<style>

</style>
