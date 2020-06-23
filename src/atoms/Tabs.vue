<template lang="pug">
  div.spt-tabs
    ul.this-tabs
      li.this-tab(v-for="(tab, i) in tabs" :key="i" @click="changeTab(i)" :class="tabCssClass(i)")
        vnode(:vnode="tab")
    div.this-tab-body(v-for="(body, i) in bodys" :key="i" :class="isHidden(i)")
      vnode(:vnode="body")
</template>

<style lang="sass" scoped>
  .spt-tabs
    ul
      border-bottom: 1px solid #e2e8f0
      display: flex
      flex-direction: row
      padding: 0 .3rem
      width: 100%
    .this-tab
      border: 1px solid transparent
      cursor: pointer
      padding: .5rem 1rem
    .this-tab-active
      background: white
      border: 1px solid #e2e8f0
      border-bottom: none
      border-top-left-radius: .3rem
      border-top-right-radius: .3rem
      margin-bottom: -1px
    .this-tab-regular
      color: inherit
    .this-tab-body
      background: white
      padding: 1rem
    .this-tab-body.is-hidden
      display: none
</style>

<script>
export default {
  components: {
    Vnode: {
      functional: true,
      render: (h, ctx) => ctx.props.vnode
    }
  },
  data: () => ({
    tabs: [],
    activeTabIndex: 0
  }),
  created () {
    this.tabs = []
    this.bodys = []
    this.$slots.default.forEach((el) => {
      this.tabs.push(el.data.scopedSlots.title())
      this.bodys.push(el.data.scopedSlots.body())
    })
  },
  methods: {
    tabCssClass (tabIndex) {
      return this.activeTabIndex === tabIndex ? 'this-tab-active' : 'this-tab-regular'
    },
    changeTab (tabIndex) {
      this.activeTabIndex = tabIndex
    },
    isHidden (tabIndex) {
      if (this.activeTabIndex !== tabIndex) {
        return 'is-hidden'
      }
    }
  }
}
</script>
