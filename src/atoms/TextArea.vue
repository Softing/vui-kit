<template lang="pug">
  textarea(
    @input="handleInput"
    ).textarea
</template>

<script>
export default {
  model: {
    prop: 'value',
    event: 'input'
  },
  props: {
    value: {
      type: String,
      default: ''
    }
  },
  methods: {
    handleInput (event) {
      this.$emit('input', event.target.value)
    }
  }
}
</script>

<style scoped lang="scss">
.textarea {
  width: 100%;
  min-height: 81px;
  resize: none;
  background-color: #FFFFFF;
  border: 1px solid #FF3182;
  border-radius: 4px;
  font-size: 14px;
}
</style>
