<template lang="pug">
  div.slider(:class="[type]")
    div.slider-button.slider-button_left.slider-button-disabled
      img(
        src="~assets/images/components/ui-kit/molecules/slider/arrow-left.svg"
        @click="prevSlide")
    div(v-swiper:sptSlider="options").slider__wrapper
      div.swiper-wrapper
        div.swiper-slide(
          v-for="(item, index) in slides"
          :key="index")
          slot(:item="item")
    div.slider-button.slider-button_right
      img(
        src="~assets/images/components/ui-kit/molecules/slider/arrow-right.svg"
        @click="nextSlide")
</template>

<script>
export default {
  props: {
    options: {
      type: Object,
      required: true
    },
    slides: {
      type: Array,
      required: true
    },
    type: {
      type: String,
      default: 'default'
    }
  },
  methods: {
    nextSlide () {
      this.sptSlider.slideNext()
    },
    prevSlide () {
      this.sptSlider.slidePrev()
    }
  }
}
</script>

<style scoped lang="scss">
.sliderImages {
  max-height: 240px;

  .swiper-slide-active {
    max-width: fit-content;
  }
}

.default {
  .swiper-slide-active {
    @include media-breakpoint-down(sm) {
      max-width: fit-content;
    }
  }

  .swiper-slide-next,
  .swiper-slide-next + .swiper-slide{
    @include media-breakpoint-down(sm) {
      max-width: fit-content;
    }
  }
}

.swiper-container {
  margin-left: -3px;
  margin-right: -3px;
  padding: 3px;
}

.slider {
  display: flex;
  flex: 1 1 auto;
  max-width: 952px;
  width: 100%;
  margin: 0 auto;

  .slider__wrapper {
    flex: 1 1 auto;
    border-radius: 4px;
  }

  .slider-button {
    flex: 0 1 auto;
    display: none;
    align-items: center;

    img {
      opacity: .5;
      cursor: pointer;

      &:hover {
        opacity: .25;
      }
    }

    &.slider-button-disabled {
      img {
        opacity: .25;
        cursor: auto;
        pointer-events: none;
      }
    }

    @include media-breakpoint-up(md) {
      display: flex;
    }

    img {
      height: 73px;
      width: 19px;
    }

    &_left {
      padding-right: 1.29rem;
    }

    &_right {
      padding-left: 1.29rem;
    }
  }
}
</style>
