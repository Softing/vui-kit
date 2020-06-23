<template lang="pug">
  div.spt-set(:class="elementCss")
    slot
</template>

<style lang="sass" scoped>
  // Default class
  .spt-set
    display: flex
    flex-wrap: wrap

    // Generate media-query values
    @each $key, $value in $spt-grid-breakpoints
      @for $i from 1 through 12
        &.#{$key}\:spt-set-cols-#{$i} > ::v-deep *
          @media (min-width: $value)
            width: calc(100% / #{$i})
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
      default: 'xs:3'
    }
  },
  computed: {
    elementCss () {
      let classes = []

      // Columns
      classes = classes.concat(this.mediaQueryHandler(this.cols, 'spt-set-cols'))

      return classes.join(' ')
    }
  }
}
</script>
