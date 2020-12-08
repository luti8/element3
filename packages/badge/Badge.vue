<template>
  <div class="el-badge">
    <slot></slot>
    <transition name="el-zoom-in-center">
      <sup
        v-show="!hidden && (content || content === 0 || isDot)"
        v-text="content"
        class="el-badge__content"
        :class="classes"
      >
      </sup>
    </transition>
  </div>
</template>

<script>
import { computed, toRefs, getCurrentInstance } from 'vue'
export default {
  name: 'ElBadge',

  props: {
    value: [String, Number],
    max: Number,
    isDot: Boolean,
    hidden: Boolean,
    type: {
      type: String,
      validator(val) {
        return (
          ['primary', 'success', 'warning', 'info', 'danger'].indexOf(val) > -1
        )
      }
    }
  },

  setup(props) {
    const { ctx } = getCurrentInstance()
    const { isDot, max, value } = toRefs(props)
    const content = useContent({ isDot, max, value })
    const classes = useClasses({ props, isDot, ctx })

    return {
      content,
      classes
    }
  }
}

const useClasses = ({ props, isDot, ctx }) => {
  return computed(() => {
    return [
      'el-badge__content--' + props.type,
      {
        'is-fixed': ctx.$slots.default,
        'is-dot': isDot
      }
    ]
  })
}

const useContent = ({ isDot, max, value }) => {
  return computed(() => {
    if (isDot.value) return
    if (
      max &&
      typeof value.value === 'number' &&
      typeof max.value === 'number'
    ) {
      return max.value < value.value ? `${max.value}+` : value.value
    }
    return value.value
  })
}
</script>
