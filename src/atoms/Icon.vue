<template lang="pug">
  i.spt-icon(:class="`icon-${icon}`")
</template>

<script>
export default {
  props: {
    icon: {
      type: String,
      default: null
    }
  }
  // computed: {
  //   elementCss () {
  //     const classes = []
  //     classes.push(`nature-${this.nature}`)
  //     return classes.join(' ')
  //   }
  // }
}
</script>
