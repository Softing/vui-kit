<template lang="pug">
  input(
    :type="type"
    :class="getClass"
    :value="currentValue"
    :cheked="checked"
    @change="handleChange"
    @input="handleInput"
    ).spt-input
</template>

<script>
export default {
  props: {
    value: {
      type: [String, Number, Array],
      default: ''
    },
    checked: {
      type: Boolean,
      default: false
    },
    modelExtractor: {
      type: Function,
      default: val => val
    },
    modelInserter: {
      type: Function,
      default: (model, val) => val
    },
    invalid: {
      type: Boolean,
      default: false
    },
    type: {
      type: String,
      required: true
    }
  },
  data: () => ({
    currentValue: null
  }),
  computed: {
    getClass () {
      return this.invalid
        ? `invalid ${this.type}`
        : `${this.type}`
    }
  },
  watch: {
    value: {
      handler (val) {
        this.currentValue = this.modelExtractor(val)
      },
      immediate: true
    }
  },
  methods: {
    handleChange (event) {
      this.$emit('change', event.target)
    },
    handleInput (event) {
      this.$emit('input', this.modelInserter(this.value, event.target.value))
    }
  }
}
</script>

<style scoped lang="sass">
.spt-input
  &.checkbox
    display: none

  &.radio
    display: none

  &.range

  &.text,
  &.password
    padding: 0 10px

  &.text
    &.invalid
      border: 1px solid rgba(255, 86, 89, 0.7) !important
</style>
