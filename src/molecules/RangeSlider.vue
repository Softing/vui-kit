<template lang="pug">
  div(:class="getClass").range-slider
    range-slider-line(
      ref="slider"
      @click.native="handleTrackClick"
      ).range-slider-line
      div(v-if="isSizeMatchRange").range-slider-line__marks
        div.range-slider-line__marks-item.item-1
        div.range-slider-line__marks-item.item-2
        div.range-slider-line__marks-item.item-3
        div.range-slider-line__marks-item.item-4
        div.range-slider-line__marks-item.item-5
      range-slider-thumb(
        v-if="isRange"
        id="additionalThumb"
        ref="addThumb"
        :style="getThumbLeft('additionalThumb', value)"
        @touchstart.native.stop="handleTouchStart"
        @mousedown.native.self="handleMouseDown").range-slider-thumb
      range-slider-thumb(
        id="primaryThumb"
        ref="thumb"
        :style="getThumbLeft('primaryThumb', value)"
        @touchstart.native.stop="handleTouchStart"
        @mousedown.native.stop="handleMouseDown").range-slider-thumb
</template>

<script>
// Components
import RangeSliderLine from '@/components/ui-kit/atoms/RangeSliderLine'
import RangeSliderThumb from '@/components/ui-kit/atoms/RangeSliderThumb'

// Types
const RangeSliderTypes = {
  Single: 'single',
  Double: 'double',
  SizeMatch: 'sizeMatch'
}

const RangeSliderOptions = {
  [RangeSliderTypes.Single]: {
    className: 'single-range',
    handleSliderClick: true
  },
  [RangeSliderTypes.Double]: {
    className: 'double-range',
    handleSliderClick: false
  },
  [RangeSliderTypes.SizeMatch]: {
    className: 'size-match-range',
    handleSliderClick: true
  }
}

const ThumbTypes = {
  Additional: 'additionalThumb',
  Primary: 'primaryThumb'
}

export const ThumbOptions = {
  [ThumbTypes.Additional]: {
    id: 'additionalThumb',
    ref: 'addThumb',
    cortegeIndex: 0
  },
  [ThumbTypes.Primary]: {
    id: 'primaryThumb',
    ref: 'thumb',
    cortegeIndex: 1
  }
}

export default {
  components: {
    RangeSliderThumb,
    RangeSliderLine
  },
  props: {
    variant: {
      type: String,
      default: RangeSliderTypes.Single,
      validator (type) {
        return !!RangeSliderOptions[type]
      }
    },
    disable: {
      type: Boolean,
      default: false
    },
    value: {
      type: [Array, Number],
      default: null
    },
    range: {
      type: Array,
      default: () => ([0, 100]),
      validator (val) {
        return val.length === 2
      }
    },
    initial: {
      type: Number,
      default: 80
    }
  },
  data: () => ({
    limit: null,
    grab: null,
    currentThumb: ThumbOptions[ThumbTypes.Primary]
  }),
  computed: {
    isRange () {
      return this.variant === RangeSliderTypes.Double
    },
    isSizeMatchRange () {
      return this.variant === RangeSliderTypes.SizeMatch
    },
    isPrimThumb () {
      return this.currentThumb.ref === ThumbOptions[ThumbTypes.Primary].ref
    },
    isAddThumb () {
      return this.currentThumb.ref === ThumbOptions[ThumbTypes.Additional].ref
    },
    hasModel () {
      return !!this.value
    },
    getClass () {
      return RangeSliderOptions[this.variant].className
    },
    getThumbLeft () {
      const { isRange, hasModel, initial } = this

      return (thumbName, value) => {
        const currThumb = ThumbOptions[thumbName]
        let thumbValue

        if (!isRange && !hasModel && initial > 0) {
          thumbValue = initial
        } else {
          thumbValue = isRange ? value[currThumb.cortegeIndex] : value
        }

        const position = this.getPositionFromValue(thumbValue)
        const coords = this.coordinates(position)

        return {
          left: `${coords.thumb}px`
        }
      }
    }
  },
  mounted () {
    this.initData()
  },
  methods: {
    initData () {
      const { thumb, slider } = this.$refs
      const sliderWidth = slider.$el.offsetWidth
      const thumbWidth = thumb.$el.offsetWidth

      this.limit = sliderWidth - thumbWidth
      this.grab = thumbWidth / 2
    },
    emitChange (val) {
      const { isRange, value, currentThumb } = this

      if (val === value[currentThumb.cortegeIndex]) { return }

      isRange
        ? this.$emit('input', value.map((a, i) => i === currentThumb.cortegeIndex ? val : a))
        : this.$emit('input', val)
    },
    setCurrentThumbName (event) {
      this.currentThumb = ThumbOptions[event.target.id]
    },
    clamp (value, min, max) {
      return Math.min(Math.max(value, min), max)
    },
    getPositionFromValue (value) {
      const { limit, range } = this

      const diffMaxMin = range[1] - range[0]
      const diffValMin = value - range[0]
      const percentage = diffValMin / diffMaxMin

      return Math.round(percentage * limit)
    },
    getValueFromPosition (pos) {
      const { limit, range } = this

      const percentage = this.clamp(pos, 0, limit) / (limit || 1)
      const baseVal = Math.round(percentage * (range[1] - range[0]))
      const value = baseVal + range[0]

      return this.clamp(value, range[0], range[1])
    },
    position (e) {
      const { grab, limit, isPrimThumb, isAddThumb, isRange } = this

      const offsetLeft = {
        add: (this.$refs.addThumb && this.$refs.addThumb.$el.offsetLeft) || null,
        prim: this.$refs.thumb.$el.offsetLeft
      }

      const range = {
        min: isRange && isPrimThumb ? offsetLeft.add - grab : 0,
        max: isRange && isAddThumb ? offsetLeft.prim - grab : limit
      }

      const coordinate = !e.touches
        ? e.clientX
        : e.touches[0].clientX

      const node = this.$refs.slider.$el
      const direction = node.getBoundingClientRect().left
      const pos = coordinate - direction - grab
      const clampedPos = this.clamp(pos, range.min, range.max)

      return this.getValueFromPosition(clampedPos)
    },
    coordinates (pos) {
      const { grab } = this

      const value = this.getValueFromPosition(pos)
      const position = this.getPositionFromValue(value)
      const thumbPos = position + grab

      return {
        thumb: thumbPos
      }
    },
    handleMove (event) {
      if (this.disable) { return }

      const value = this.position(event)

      this.emitChange(value)
    },
    handleTrackClick (event) {
      if (this.disable || this.isRange) { return }

      this.handleMove(event)
    },
    handleMouseUp () {
      document.removeEventListener('mousemove', this.handleMove)
      document.removeEventListener('mouseup', this.handleMouseUp)
    },
    handleMouseDown (event) {
      if (this.disable) { return }

      if (this.isRange) { this.setCurrentThumbName(event) }

      document.addEventListener('mousemove', this.handleMove)
      document.addEventListener('mouseup', this.handleMouseUp)
    },
    handleTouchEnd () {
      document.removeEventListener('touchmove', this.handleMove)
      document.removeEventListener('touchend', this.handleTouchEnd)
    },
    handleTouchStart (event) {
      if (this.disable) { return }

      if (this.isRange) { this.setCurrentThumbName(event) }

      document.addEventListener('touchmove', this.handleMove)
      document.addEventListener('touchend', this.handleTouchEnd)
    }
  }
}
</script>

<style scoped lang="scss">
.range-slider {
  width: 100%;
  display: flex;
  align-items: center;

  &.single-range,
  &.double-range {
    height: 25px;

    .range-slider-line {
      background-color: rgba(62, 69, 77, .1);
      height: 5px;
    }

    .range-slider-thumb {
      height: 25px;
      width: 25px;
      border-radius: 50%;
      background-color: #FFFFFF;
      border: 1px solid #D0D0D0;
    }
  }

  &.size-match-range {
    height: 12px;

    .range-slider-line {
      background-color: rgba(62, 69, 77, 0.1);
      height: 6px;
      border-radius: 0;
    }

    .range-slider-line__marks {
      width: 100%;
      position: absolute;
      top: -3px;

      .range-slider-line__marks-item {
        position: absolute;
        background-color: #E3DED4;
        border-radius: 3px;
        width: 2px;
        height: 12px;

        @for $i from 0 through 4 {
          &.item-#{$i} {
            left: calc(25% * #{$i});
          }
        }
      }
    }

    .range-slider-thumb {
      height: 12px;
      width: 12px;
      border-radius: 50%;
      background-color: #FF3182;
    }
  }
}
</style>
