<template lang="pug">
  div.price-filter
    div.price-filter__inputs
      spt-input(
        type="text"
        v-model="inputs"
        :invalid="isInvalidInputs.first"
        :model-extractor="val => val[0]"
        :model-inserter="(model, val) => this.updateModelValues(model,val,0)"
        ).price-filter__inputs-item
      span.price-filter__inputs-delimiter -
      spt-input(
        type="text"
        v-model="inputs"
        :invalid="isInvalidInputs.second"
        :model-extractor="val => val[1]"
        :model-inserter="(model, val) => this.updateModelValues(model,val,1)"
        ).price-filter__inputs-item
      spt-icon(icon="ruble").price-filter__inputs-currency
      spt-button(
        nature="width"
        @click.native="submit"
        ).price-filter__inputs-submit
        span.price-filter__inputs-submit-text ок
    range-slider(
      variant="double"
      v-model="slider"
      :range="range"
      ).price-filter-slider
</template>

<script>
import RangeSlider from '@/components/ui-kit/molecules/RangeSlider'

export default {
  components: { RangeSlider },
  props: {
    range: {
      type: Array,
      default: () => [0, 100],
      validator (val) {
        return val.length === 2
      }
    }
  },
  data: () => ({
    isInvalidInputs: {
      first: false,
      second: false
    },
    slider: null,
    inputs: null
  }),
  watch: {
    range: {
      handler (val) {
        this.slider = val
        this.inputs = val
      },
      immediate: true
    },
    slider: {
      handler (val) {
        this.inputs = val
      }
    },
    inputs: {
      handler (val) {
        const isInvalidVal = a => typeof a === 'number' && !(a >= this.range[0] && a <= this.range[1])

        this.isInvalidInputs = {
          first: isInvalidVal(val[0]),
          second: isInvalidVal(val[1])
        }
      }
    }
  },
  methods: {
    updateModelValues: (arr, val, i) => arr.map((a, j) => i === j ? +val : +a),
    submit () {
      if (!this.isInvalidInputs.first && !this.isInvalidInputs.second) {
        this.slider = this.inputs
        this.$emit('input', this.inputs)
      }
    }
  }
}
</script>

<style scoped lang="scss">
.price-filter {
  .price-filter__inputs {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    color: rgba(62, 69, 77, .6);

    .price-filter__inputs-item {
      width: 100%;
      height: 25px;
      background-color: #FFFFFF;
      border: 1px solid #D0D0D0;
      border-radius: 4px;
      font-size: 10px;
      color: rgba(62, 69, 77, .6);
      padding: 0 5px;
    }

    .price-filter__inputs-currency {
      margin: 0 5px;
      font-size: 10px;
    }

    .price-filter__inputs-delimiter {
      font-size: 16px;
      margin: 0 4px;
    }

    .price-filter__inputs-submit {
      background-color: #F1EFEF;
      border: 1px solid #D0D0D0;
      padding: 0 3px 2px;
      height: 25px;

      .price-filter__inputs-submit-text {
        line-height: 14px;
        font-size: 16px;
        font-weight: 600;
        color: rgba(62, 69, 77, .6);
      }
    }
  }
}
</style>
