<template lang="pug">
  li.spt-tree-node
    div.spt-tree-node__root
      div(
        :class="{'spt-tree-node__content_hasChildren': hasChildren}"
        @click="handleNodeClick"
        ).spt-tree-node__content
        span(
          :class="{'spt-tree-node-title_root': isRoot}"
          ).spt-tree-node-title
          slot(
            name="node"
            :node="node"
            )
      ul(v-if="hasChildren && isChildrenShow").spt-tree-node__children
        node(
          v-for="(n, i) in node.children"
          :key="i"
          :node="n"
          :isRoot="false"
        )
          template(#node="{node}")
            slot(
              name="node"
              :node="node"
            )
</template>

<script>
export default {
  name: 'Node',
  props: {
    node: {
      type: Object,
      required: true
    },
    isRoot: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    hasChildren () {
      return this.node.children && this.node.children.length
    },
    isChildrenShow () {
      return this.node.options.opened
    }
  },
  methods: {
    handleNodeClick () {
      this.node.options.opened = !this.node.options.opened
    }
  }
}
</script>

<style scoped lang="sass">
.spt-tree-node

  .spt-tree-node__root

    .spt-tree-node__content

      &.spt-tree-node__content_hasChildren
        margin-bottom: .3rem

      .spt-tree-node-title
        font-size: 16px
        line-height: 28px
        color: #888D92

        &.spt-tree-node-title_root
          color: rgba(136, 141, 146, 0.6)

    .spt-tree-node__children
      padding-left: 1.2rem
</style>
