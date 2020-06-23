<template lang="pug">
  button.spt-button(:class="elementCss" @click="$emit('click')")
    i(:class="`icon-${icon}`" v-if="icon")
    span(v-if="$slots.default")
      slot
</template>

<style lang="sass" scoped>

  .spt-button
    background: #FFFFFF
    box-shadow: rem(0px) rem(1px) rem(3px) rgba(0, 0, 0, 0.15)
    border: rem(1px) solid rgba(0, 0, 0, 0.15)
    border-radius: 4px
    cursor: pointer
    display: inline-flex
    align-items: center
    padding: .625rem .5rem .625rem .5rem;
    position: relative
    /*margin: .0625rem .09375rem .0625rem .09375rem */

    i
      align-items: center
      display: flex
      font-size: rem(13px)
      justify-content: center
      height: rem(16px)
      /*margin: 0 .3rem*/
      width: rem(28px)
      color: #B1A8B9

    span
      display: inline-flex
      align-items: center
      justify-content: center
      font-size: em(14px)
      line-height: rem(16px)
      margin: 0 .3rem
      overflow: hidden
      text-align: left
      text-overflow: ellipsis
      width: 100%
      white-space: nowrap

    &.nature-icon
      i
        font-size: em(16px)
        color: #fff

    &.nature-width
      span
        width: auto
        margin: 0

    &.secondary
      background: linear-gradient(0deg, #F79400 0.36%, #FFAB2D 99.42%)
      border: 1px solid #CD6200

    &:active, &:hover
      background: #F5EFFF
      border: rem(1px)   solid #9D6CE9
      /*box-shadow: rem(0px) rem(1px) rem(3px) rgba(0, 0, 0, 0.15)*/

    &.primary
      background: linear-gradient(0deg, #F7B500 0.36%, #FFC72D 99.42%)
      border: 1px solid #CD9600

    &.big
      padding: 0.9rem 1.1rem
      font-weight: 600

      i
        font-size: em(20px)
        opacity: .5

</style>

<script>
export default {
  props: {
    icon: {
      type: String,
      default: null
    },
    nature: {
      type: String,
      default: 'text'
    }
  },
  computed: {
    elementCss () {
      const classes = []
      classes.push(`nature-${this.nature}`)
      return classes.join(' ')
    }
  }
}
</script>
