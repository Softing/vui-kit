<template lang="pug">
  spt-columns(wrap="nowrap").social-rating
    spt-column(flex="0").social-rating-rate
      spt-icon(icon="thumbs-up").icon
      span.count {{ data.like }}
    spt-column(flex="0").social-rating-rate
      spt-icon(icon="thumbs-down").icon
      span.count {{ data.dislike }}
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      default: () => ({
        like: 0,
        dislike: 0
      })
    }
  }
}
</script>

<style scoped lang="scss">
.social-rating {
  align-items: center;
  .social-rating-rate {
    .icon {
      color: rgba(177, 168, 185, 1);
      margin-right: 8px;
    }

    .count {
      color: #3E454D;
    }

    &:first-child {
      margin-right: 35px;
    }
  }
}
</style>
