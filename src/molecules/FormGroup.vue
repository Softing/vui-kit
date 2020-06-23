<template lang="pug">
  div.spt-form-group
    slot
</template>

<style scoped lang="sass">
.spt-form-group
  display: flex
  flex-direction: column
  align-items: flex-start

  & > *
    &:not(:last-child)
      margin-bottom: 3px
</style>
