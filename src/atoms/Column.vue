<template lang="pug">
  div.spt-column(:class="elementCss")
    slot
</template>

<style lang="sass" scoped>

  .spt-column

    display: flex

    // Columns and flexes
    @each $key, $value in $spt-grid-breakpoints
      @for $i from 0 through 12
        // Flex
        &.#{$key}\:spt-column-flex-#{$i}
          @media (min-width: $value)
            flex: #{$i} 0 auto

        // Cols
        &.#{$key}\:spt-column-cols-#{$i}
          @media (min-width: $value)
            width: percentage(1/12 * $i)

    // Center
    @each $breakpointKey, $breakpointValue in $spt-grid-breakpoints
      &.#{$breakpointKey}\:spt-column-align-center
        @media (min-width: $breakpointValue)
          justify-content: center
          align-items: center

    // Horizontal align
    $aligns: (left flex-start, right flex-end, center center)
    @each $breakpointKey, $breakpointValue in $spt-grid-breakpoints
      @each $alignKey, $alignValue in $aligns
        &.#{$breakpointKey}\:spt-column-align-h-#{$alignKey}
          @media (min-width: $breakpointValue)
            justify-content: $alignValue

    // Vertical align
    $aligns: (top flex-start, bottom flex-end, center center)
    @each $breakpointKey, $breakpointValue in $spt-grid-breakpoints
      @each $alignKey, $alignValue in $aligns
        &.#{$breakpointKey}\:spt-column-align-v-#{$alignKey}
          @media (min-width: $breakpointValue)
            align-items: $alignValue

</style>

<script>
import uiKitMixins from '@/components/ui-kit/Mixins'

export default {
  mixins: [
    uiKitMixins
  ],
  props: {
    align: {
      type: String,
      default: 'top:left'
    },
    cols: {
      type: String,
      default: null
    },
    flex: {
      type: String,
      default: 'xs:1'
    }
  },
  computed: {
    elementCss () {
      let classes = []

      if (this.cols) { // Width
        classes = classes.concat(this.mediaQueryHandler(this.cols, 'spt-column-cols'))
      }

      if (this.flex) { // Flex
        classes = classes.concat(this.mediaQueryHandler(this.flex, 'spt-column-flex'))
      }

      // Align
      classes = classes.concat(this.mediaQueryHandler(this.align, 'spt-column-align'))

      return classes.join(' ')
    }
  }
}
</script>
