<template lang="pug">
  div(:class="userGrade").user-grade
    img(:src="getImgSrc")
    div.grade
      span {{ getUserGrade }}
      span ({{ userGradeRate }})
</template>

<script>
import { UserGradeTypes } from '@/types/ComponentTypes'

const HumanReadableUserGrades = {
  [UserGradeTypes.Junior]: 'Новичок',
  [UserGradeTypes.Expert]: 'Опытный',
  [UserGradeTypes.Master]: 'Мастер'
}

export default {
  props: {
    userGrade: {
      type: String,
      required: true
    },
    userGradeRate: {
      type: String,
      required: true
    }
  },
  computed: {
    getImgSrc () {
      return require(`@/assets/images/components/application/components/category/offer/userGrades/${this.userGrade}-grade.svg`)
    },
    getUserGrade () {
      if (!HumanReadableUserGrades[this.userGrade]) {
        console.warn('UserGradeWarn: You passed unknown user grade type')
        return ''
      }

      return HumanReadableUserGrades[this.userGrade]
    }
  }
}
</script>

<style scoped lang="scss">
.user-grade {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 12px;
  height: 21px;
  padding-left: 4px;
  padding-right: 5px;

  @include media-breakpoint-up(md) {
    height: 27px;
    width: 138px;
    padding-left: 8px;
    padding-right: 6px;
  }

  img {
    height: 15px;
    margin-right: 2px;

    @include media-breakpoint-up(md) {
      margin-right: 7px;
    }
  }

  .grade {
    display: flex;

    span {
      font-size: 16px;
      color: #3E454D;

      &:first-child {
        margin-right: .2rem;
      }
    }
  }

  &.junior {
    border: 1px solid #EEEEEE;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.15);
    border-radius: 12px;
  }

  &.expert {
    background: #FBEFE2;
    border: 1px solid #DFCCB0;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.15);
  }

  &.master {
    background: #FFF9EC;
    border: 1px solid #F7B500;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.15);
  }
}
</style>
