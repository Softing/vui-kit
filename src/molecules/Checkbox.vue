<template lang="pug">
  spt-label.spt-checkbox
    spt-input(
      type="checkbox"
      :value="value"
      :checked="shouldBeChecked"
      @change="updateInput"
      @input="handleInput"
      )
    span.label {{ label }}
</template>

<script>
export default {
  model: {
    prop: 'modelValue',
    event: 'change'
  },
  props: {
    value: {
      type: String,
      default: ''
    },
    modelValue: {
      type: [Array, Boolean, String, Number],
      default: false
    },
    label: {
      type: String,
      required: true
    },
    trueValue: {
      type: Boolean || String,
      default: true
    },
    falseValue: {
      type: Boolean || String,
      default: false
    }
  },
  computed: {
    shouldBeChecked () {
      if (Array.isArray(this.modelValue)) {
        return this.modelValue.includes(this.value)
      }
      return this.modelValue === this.trueValue
    }
  },
  methods: {
    handleInput (e) {
      this.$emit('input', e)
    },
    updateInput (event) {
      const isChecked = event.checked

      if (Array.isArray(this.modelValue)) {
        const newValue = [...this.modelValue]

        isChecked
          ? newValue.push(this.value)
          : newValue.splice(newValue.indexOf(this.value), 1)

        this.$emit('change', newValue)
      } else {
        this.$emit('change', isChecked ? this.trueValue : this.falseValue)
      }
    }
  }
}
</script>

<style scoped lang="sass">
.spt-checkbox
  display: flex
  align-items: center
  height: 25px
  font-size: 16px
  color: rgba(62, 69, 77, .6)
  line-height: 28px

  .label
    position: relative
    padding: 0 0 0 35px
    cursor: pointer
    height: 100%
    display: flex
    align-items: center

    &:before
      content: ''
      position: absolute
      font-size: 0em
      top: 0px
      left: 0px
      width: 25px
      height: 25px
      transition: .2s ease-in
      transition-property: background-color, font-size
      background: #FFFFFF
      border: 1px solid #D0D0D0
      box-sizing: border-box
      border-radius: 4px

  .spt-input:checked + .label:before
    content: '\e91f'
    font-family: 'spt'
    color: #ffffff
    font-size: .8em
    background: #0071CB
    border: none
    display: flex
    align-items: center
    justify-content: center
</style>
