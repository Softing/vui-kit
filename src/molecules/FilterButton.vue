<template lang="pug">
  spt-button(
    nature="width"
    icon="filter"
    @click.native="handleClick"
    ).spt-filter-button
    div(v-show="total > 0").spt-filter-button-total {{ total }}
</template>

<script>
// Store imports
import { mapState } from 'vuex'

// Types
import { FilterViewTypes, FilterViewOptions } from '@/types/ComponentTypes'

export default {
  props: {
    type: {
      type: String,
      default: null,
      validator (val) {
        return !!FilterViewOptions[val]
      }
    }
  },
  computed: {
    ...mapState({
      state: state => state.filter.openFilter,
      total: state => state.filter.count
    })
  },
  watch: {
    state (to) {
      document.body.style.overflow = to && to === FilterViewTypes.List
        ? 'hidden'
        : 'auto'
    }
  },
  methods: {
    handleClick () {
      const { state, type } = this
      const menuState = state === FilterViewTypes.List || state === FilterViewTypes.Card
        ? null
        : type

      this.$store.dispatch('filter/setOpenFilters', menuState)
    }
  }
}
</script>

<style scoped lang="sass">
.spt-filter-button
  position: relative
  height: 36px
  width: 36px
  font-size: .9em

  .spt-filter-button-total
    display: flex
    justify-content: center
    align-items: center
    position: absolute
    top: -5px
    right: -4px
    height: 14px
    width: 14px
    background-color: #FF3131
    border-radius: 50%
    font-size: 10px
    text-align: center
    color: #FFFFFF
</style>
