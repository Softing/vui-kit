<template lang="pug">
  div.spt-tab(:class="elementCss")
    slot(name="title")
    slot(name="body")
</template>

<style lang="sass" scoped>
  .spt-tab
    display: flex
</style>

<script>
export default {
  props: {
    align: {
      type: String,
      default: 'top:left'
    }
  },
  computed: {
    elementCss () {
      const classes = []
      return classes.join(' ')
    }
  }
}
</script>
