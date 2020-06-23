<template lang="pug">
  div.spt-card
    header(v-if="$slots.header")
      slot(name="header")
    main
      slot
    footer(v-if="$slots.footer")
      slot(name="footer")
</template>

<style lang="sass" scoped>
  .spt-card
    background: #FFFFFF
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.15)
    border-radius: 4px
</style>
