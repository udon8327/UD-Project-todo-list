<template>
  <div class="ud-switch">
    <label>
      <input 
        type="checkbox"
        v-model="modelValue"
        v-bind="$attrs"
      >
      <div class="switch-decorator">
        <div class="circle"></div>
      </div>
      <slot></slot>
    </label>
  </div>
</template>

<script>
export default {
  name: 'UdSwitch',
  inheritAttrs: false,
  props: {
    value: { default: false }, // value值
  },
  computed: {
    modelValue: {
      get(){ return this.value },
      set(val){ this.$emit('input', val) }
    }
  }
}
</script>

<style lang="sass" scoped>
.ud-switch
  label
    cursor: pointer
    display: flex
    align-items: center
    min-height: 40px
    input
      position: absolute
      left: -999px
      opacity: 0
      pointer-events: none
      &:checked + .switch-decorator
        background: $main
        .circle
          margin-left: 20px
    .switch-decorator
      width: 40px
      height: 20px
      border-radius: 20px
      background: #ccc
      transition: all .2s ease
      padding-left: 2px
      margin-right: 6px
      display: flex
      align-items: center
      .circle
        width: 16px
        height: 16px
        border-radius: 50%
        background: #f7f7f7
        transition: all .2s ease
</style>
