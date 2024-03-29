<template>
  <div class="ud-textarea">
    <textarea
      ref="textarea"
      v-model="modelValue"
      v-bind="$attrs"
      v-on="inputListeners"
      :rows="rows"
      :class="{ 'is-no-resize': noResize }"
      @input="onInput"
      :maxlength="limit"
    >
    </textarea>
    <div class="textarea-limit" v-if="showLimit" :class="{ 'limit-input': value.length > 0 }">
      <span>{{ valueLength }}/{{ limit }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UdTextarea',
  inheritAttrs: false,
  props: {
    value: null,
    rows: { default: 4 }, // 行數
    showLimit: Boolean, // 是否顯示字數限制
    limit: { default: null }, // 字數限制
    noResize: Boolean // 禁止改變大小
  },
  computed: {
    modelValue: {
      get(){ return this.value == null ? "" : this.value },
      set(val){ this.$emit('input', val) }
    },
    valueLength() {
      return this.value.length;
    },
    inputListeners() {
      return Object.assign({},
        this.$listeners,
        { input: event => {} }
      )
    }
  },
  methods: {
    onInput() {
      this.$parent.$emit('validate'); // 通知FormItem校驗
    },
    focus() {
      this.$refs.textarea.focus();
    },
    blur() {
      this.$refs.textarea.blur();
    }
  }
}
</script>

<style lang="sass" scoped>
.ud-textarea
  position: relative
  textarea
    appearance: none
    width: 100%
    padding: 5px 10px
    min-height: 40px
    font-size: 14px
    color: #000
    border: 1px solid #ccc
    border-radius: 4px
    background-color: #fff
    transition: all 0.2s ease
    &.is-no-resize
      resize: none
    &::placeholder
      color: #aaa
    &:focus
      outline: 0
      border: 1px solid #000
    &:disabled
      background-color: #efefef
      cursor: not-allowed
  .textarea-limit
    position: absolute
    right: 0px
    bottom: -20px
    span
      color: #ccc
      letter-spacing: 1px
      font-size: 14px
    &.limit-input
      span
        color: #000
</style>
