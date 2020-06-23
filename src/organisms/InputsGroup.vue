<template lang="pug">
  spt-columns(:class="view").inputs-group
    spt-column(
      v-for="(input, i) in inputs"
      :key="i"
      flex="1"
    ).inputs-group-item
      slot(
        name="input"
        :input="input"
        :id="i"
        )
</template>

<script>
export default {
  props: {
    inputs: {
      type: Array,
      required: true
    },
    view: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped lang="scss">
.inputs-group {
  &.list {
    flex-direction: column;

    .inputs-group-item {
      &:not(:last-child) {
        margin-bottom: 3px;
      }
    }
  }

  &.inline {
    flex-direction: row;

    .inputs-group-item {
      max-width: min-content;

      &:not(:last-child) {
        margin-right: 8px;
      }
    }
  }
}
</style>
