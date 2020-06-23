<template lang="pug">
  div.spt-rating-stars
    div(
      v-for="(item, i) in starsArray"
      :key="i"
      @click="handleClick(i)"
      ).spt-rating-star
      spt-icon(
        icon="star"
        :class="getStateClass(i, starsCount)"
        )
    a(v-if="hasReviewsCount").spt-rating-reviews-count {{ reviewsCount }}
</template>

<script>
export default {
  model: {
    prop: 'rate',
    event: 'input'
  },
  props: {
    rate: {
      type: String,
      default: '0'
    },
    reviewsCount: {
      type: String,
      default: '0'
    },
    set: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    starsCount: 0,
    starsArray: new Array(5)
  }),
  computed: {
    getStateClass () {
      return (index, stars) => {
        return ++index <= stars ? 'active' : 'unactive'
      }
    },
    hasReviewsCount () {
      return +(this.reviewsCount) > 0
    }
  },
  watch: {
    rate: {
      handler (val) {
        this.starsCount = +val
      },
      immediate: true
    }
  },
  methods: {
    handleClick (index) {
      if (this.set) {
        this.starsCount = ++index
        this.$emit('input', this.starsCount)
      }
    }
  }
}
</script>

<style scoped lang="scss">
.spt-rating-stars {
  display: flex;
  max-width: 91px;
  font-size: 1.05em;

  .spt-rating-star {

    &:not(:last-child) {
      margin-right: 2px;
    }

    .spt-icon {
      position: relative;

      &.active {
        color: #F7B500;
      }

      &.unactive {
        color: rgba(53, 55, 59, .1);
      }
    }
  }

  .spt-rating-reviews-count {
    margin-left: 11px;
    font-size: 16px;
    line-height: 19px;
    color: #0071CB;
  }
}
</style>
