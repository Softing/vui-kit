<template lang="pug">
  spt-button(
    :class="getOptionClass"
    @click.native="handleClick"
    ).text-button {{ value }}
</template>

<script>
import { TextButtonTypes } from '@/types/ComponentTypes'

const ButtonOptions = {
  [TextButtonTypes.Fit]: {
    type: 'fit'
  },
  [TextButtonTypes.Normal]: {
    type: 'normal'
  }
}

export default {
  model: {
    prop: 'model',
    event: 'input'
  },
  props: {
    type: {
      type: String,
      default: TextButtonTypes.Normal
      /* validator (type) {
        return !!ButtonOptions[type]
      } */
    },
    model: {
      type: String,
      default: null
    },
    value: {
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
  data: () => ({
    currentButtonType: TextButtonTypes.Normal
  }),
  computed: {
    getOptionClass () {
      return this.id === this.activeId
        ? `active ${this.type}`
        : `${this.type}`
    }
  },
  watch: {
    type: {
      handler (newType) {
        ButtonOptions[newType]
          ? this.currentButtonType = newType
          : console.warn(`TextButton: You passed the unknown button type: ${newType}!`)
      },
      immediate: true
    }
  },
  methods: {
    handleClick () {
      const returnedValue = this.id === this.activeId ? null : this.value

      this.$emit('input', returnedValue)
    }
  }
}
</script>

<style scoped lang="scss">
.text-button {
  background: #FFFFFF;
  border: 1px solid #D8D8D8;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
  font-size: 16px;

  &.fit {

  }

  &.normal {
    min-width: 78px;
  }

  &.active {
    background: #F5EFFF;
    border: rem(1px) solid #9D6CE9
  }
}
</style>
