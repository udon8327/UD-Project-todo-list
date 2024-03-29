<template>
  <transition name="fade">
    <div class="ud-alert" v-if="isShow" @click.self="maskHandler">
      <div class="ud-modal-wrapper">
        <div class="ud-modal-close" v-if="btnClose" @click="destroy">
          <i class="icon-close"></i>
        </div>
        <div class="ud-modal-header" v-if="title">
          <p v-html="nl2br(title)"></p>
        </div>
        <div class="ud-modal-body">
          <p v-html="nl2br(message)"></p>
        </div>
        <div class="ud-modal-footer">
          <ud-flex>
            <ud-button @click="cancelHandler" plain v-if="confirm">{{ cancelText }}</ud-button>
            <ud-button @click="confirmHandler">{{ confirmText }}</ud-button>
          </ud-flex>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { nl2br } from '@/utils/ud-utils'

export default {
  name: 'UdAlert',
  data() {
    return {
      isShow: false, // 是否顯示
      confirm: false, // 是否有確認+取消鈕
      maskClose: false, // 點擊遮罩關閉
      btnClose: false, // 右上關閉按鈕
      scrollLock: true, // 是否鎖定背景頁面捲動
      title: "", // 標題文字
      message: "", // 訊息文字
      cancelText: "取消", // 取消鈕文字
      onCancel: () => {}, // 取消鈕callback
      confirmText: "確定", // 確認鈕文字
      onConfirm: () => {}, // 確認鈕callback
      resolve: "", // 保存resolve
      reject: "", // 保存reject
    }
  },
  mounted() {
    if(this.scrollLock) document.getElementsByTagName('body')[0].style.overflow = 'hidden';
    if(this.scrollLock) document.getElementsByTagName('body')[0].style.overflowY = 'hidden';
    if(this.message === "") this.message = this.msg;
  },
  methods: {
    show() {
      this.isShow = true;
      return new Promise((resolve, reject) => {
        this.resolve = resolve;
        this.reject = reject;
      })
    },
    nl2br(val) {
      return nl2br(val);
    },
    confirmHandler() {
      this.onConfirm();
      this.resolve('confirm');
      this.destroy();
    },
    cancelHandler() {
      this.onCancel();
      this.reject('cancel');
      this.destroy();
    },
    maskHandler() {
      if(this.maskClose) this.destroy();
    },
    destroy() {
      this.isShow = false;
      if(this.scrollLock) document.getElementsByTagName('body')[0].style.overflow = 'auto'
      setTimeout(() => {
        this.$destroy(true);
        this.$el.parentNode.removeChild(this.$el);
      }, 200);
    },
  }
}
</script>

<style lang="sass" scoped>
.ud-alert
  position: fixed
  z-index: 120
  left: 0
  top: 0
  right: 0
  bottom: 0
  width: 100%
  height: 100%
  background-color: rgba(0, 0, 0, 0.5)
  display: flex
  justify-content: center
  align-items: center
  overflow-x: hidden
  .ud-modal-wrapper
    position: relative
    padding: 15px
    width: 90%
    max-width: 400px
    max-height: 88%
    background-color: #fff
    box-shadow: 0px 3px 20px 0px rgba(0, 0, 0, 0.3)
    text-align: center
    display: flex
    flex-direction: column
    p
      font-size: 16px
      margin-bottom: 0
      color: #333
    .ud-modal-close
      position: absolute
      width: 26px
      height: 26px
      right: 4px
      top: -32px
      cursor: pointer
      &:hover
        .icon-close
          opacity: 1
      .icon-close
        opacity: 0.75
        transition: all 0.2s ease
        &:before, &:after
          position: absolute
          left: 13px
          content: ''
          height: 26px
          width: 2px
          background-color: #fff
        &:before
          transform: rotate(45deg)
        &:after
          transform: rotate(-45deg)
    .ud-modal-header
      flex: 0 0 auto
      padding: 0 0 15px 0
      p
        font-size: 18px
        font-weight: bold
    .ud-modal-body
      flex: 1 1 auto
      padding: 5px 15px
      margin: 0 -15px
      overflow-y: auto
    .ud-modal-footer
      flex: 0 0 auto
      padding: 15px 0 0 0
</style>
