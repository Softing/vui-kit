<template lang="pug">
  spt-tabs.spt-code-example
    spt-tab
      template(#title) HTML
      template(#body)
        slot(name="code")
    spt-tab
      template(#title) Source
      template(#body)
        div.this-source-code
          pre
            slot(name="source")
</template>

<style lang="sass">
  .spt-code-example
    .this-source-code
      font-size: .8em
</style>
