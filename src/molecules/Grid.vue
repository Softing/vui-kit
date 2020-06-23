<template lang="pug">
  div.spt-grid(:class="elementCss")
    slot
</template>

<style lang="sass" scoped>
  .spt-grid
    display: grid

    // Generate media-query values
    @each $key, $value in $spt-grid-breakpoints
      @for $i from 1 through 12

        // Columns
        &.#{$key}\:spt-grid-cols-#{$i}
          @media (min-width: $value)
            grid-template-columns: repeat(#{$i}, 1fr)

        // Gap
        &.#{$key}\:spt-grid-gap-#{$i}
          @media (min-width: $value)
            grid-column-gap: #{$i * .3}rem
            grid-row-gap: #{$i * .3}rem
</style>

<script>
import uiKitMixins from '@/components/ui-kit/Mixins'

export default {
  mixins: [
    uiKitMixins
  ],
  props: {
    cols: {
      type: String,
      default: 'xs:4'
    },
    gap: {
      type: String,
      default: 'xs:4'
    }
  },
  computed: {
    elementCss () {
      let classes = []

      // Cols
      classes = classes.concat(this.mediaQueryHandler(this.cols, 'spt-grid-cols'))

      // Gap
      classes = classes.concat(this.mediaQueryHandler(this.gap, 'spt-grid-gap'))

      return classes.join(' ')
    }
  }
}
</script>
