<template lang="pug">
  div.spt-columns(:class="elementCss")
    slot
</template>

<style lang="sass" scoped>
  .spt-columns
    display: flex
    width: 100%

    @each $key, $value in $spt-grid-breakpoints
      &.#{$key}\:spt-columns-wrap
        @media (min-width: $value)
          flex-wrap: wrap
      &.#{$key}\:spt-columns-no-wrap
        @media (min-width: $value)
          flex-wrap: nowrap
</style>

<script>
import uiKitMixins from '@/components/ui-kit/Mixins'
export default {
  mixins: [
    uiKitMixins
  ],
  props: {
    wrap: {
      type: String,
      default: 'xs:wrap'
    }
  },
  computed: {
    elementCss () {
      let classes = []

      // Wrap
      classes = classes.concat(this.mediaQueryHandler(this.wrap, 'spt-columns'))

      return classes.join(' ')
    }
  }
}
</script>
