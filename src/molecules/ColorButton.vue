<template lang="pug">
  spt-button(
    :class="{ 'active': id === activeId }"
    nature="width"
    @click.native="handleClick"
    ).color-button
    div(:class="color").color
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      default: ''
    },
    value: {
      type: String,
      default: null
    },
    color: {
      type: String,
      default: ''
    },
    id: {
      type: Number,
      default: null
    },
    activeId: {
      type: Number,
      default: null
    }
  },
  methods: {
    handleClick () {
      const returnedValue = this.id === this.activeId ? null : this.color

      this.$emit('input', returnedValue)
    }
  }
}
</script>

<style scoped lang="scss">
.color-button {
  width: auto;
  border-radius: 12px;
  border: 2px solid #FFFFFF;
  overflow: hidden;
  box-shadow: none;
  padding: 0;
  margin: 0;

  .color {
    height: 36px;
    width: 36px;
    box-shadow: 0 0 0 #9D6CE9, inset 0 3px 6px rgba(0, 0, 0, 0.150677);

    &.space-gray {
      background-color: #ABAEB1;
    }
  }

  &.active {
    background: #F5EFFF;
    border: rem(1px) solid #9D6CE9
  }
}
</style>
