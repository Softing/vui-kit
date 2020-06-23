<template lang="pug">
  ul.spt-tree
    spt-tree-node(:node="node")
      template(#node="{node}")
        slot(
          :node="node"
          name="node"
        )
</template>

<script>
export default {
  props: {
    rootNode: {
      type: Object,
      required: true
    }
  },
  data: () => ({
    node: null
  }),
  watch: {
    rootNode: {
      handler (rootNode) {
        this.node = this.makeNode(rootNode, 'root')
      },
      immediate: true
    }
  },
  methods: {
    makeNode (data, nodeType) {
      const node = this.mixinOptions(data, nodeType)
      node.children = (node.children || []).map(child => this.makeNode(child, 'child'))

      return node
    },
    mixinOptions (node, nodeType) {
      return {
        ...node,
        options: {
          opened: nodeType === 'root'
        }
      }
    }
  }
}
</script>

<style scoped lang="sass">
.spt-tree
  margin-bottom: 20px
</style>
