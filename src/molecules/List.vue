<template lang="pug">
  div.spt-list
    slot
</template>

<style lang="sass" scoped>
  .spt-list
    display: flex
    flex-direction: column
</style>

<script>
export default {
  props: {
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
