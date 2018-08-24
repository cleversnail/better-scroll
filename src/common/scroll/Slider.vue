<template>
  <div class="wrapper" ref="wrapper">
    <div class="content" ref="content">
      <slot></slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="(item, index) in dots" :key="index" :class="{active: currentPageIndex === index }"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import { addClass } from '@/common/js/dom'
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      dots: [],
      currentPageIndex: 0
    }
  },
  props: {
    // 父组件传过来的当前tab
    oCurrentPage: {
      type: Number,
      default: 0
    },
    probeType: {
      type: Number,
      default: 1
    },
    scrollX: {
      type: Boolean,
      default: true
    },
    scrollY: {
      type: Boolean,
      default: false
    },
    click: {
      type: Boolean,
      default: false
    },
    // 无限循环
    loop: {
      type: Boolean,
      default: false
    },
    // 自动轮播
    autoPlay: {
      type: Boolean,
      default: false
    },
    interval: {
      type: Number,
      default: 4000
    },
    // 边界回弹
    bounce: {
      type: Boolean,
      default: false
    }
  },
  mounted  () {
    // 保证在DOM渲染完毕后初始化better-scroll
    setTimeout(() => {
      this._initContentWidth()
      this._initScroll()
      this._initDots()
      console.log(this.oCurrentPage)
    }, 20)
    window.addEventListener('resize', () => {
      if (!this.slider) {
        return
      }
      this._initContentWidth(true)
      this.slider.refresh()
    })
    if (this.autoPlay) {
      this._play()
    }
  },
  methods: {
    // 父组件动态传值过来调用的方法
    setPage (val) {
      this.currentPageIndex = val
      this.slider.goToPage(this.currentPageIndex, 0, 400)
    },
    // 分页圆点设置
    _initDots () {
      // console.log(this)
      this.dots = new Array(this.children.length)
    },
    // 计算该组件滑动容器content的宽度
    _initContentWidth (isResize) {
      // 获取组件下有多少个子元素
      this.children = this.$refs.content.children
      console.log(this.children)
      let width = 0
      let wrapperWidth = this.$refs.wrapper.clientWidth
      console.log(wrapperWidth)
      for (let i = 0; i < this.children.length; i++) {
        // console.log(this.children[i])
        let child = this.children[i]
        addClass(child, 'slider-item')
        child.style.width = wrapperWidth + 'px'
        width += wrapperWidth
      }
      // 无缝循环轮播时
      if (this.loop && !isResize) {
        width += 2 * wrapperWidth
      }
      this.$refs.content.style.width = width + 'px'
    },
    // 实例化better-scroll函数
    _initScroll () {
      this.slider = new BScroll(this.$refs.wrapper, {
        probeType: this.probeType,
        scrollX: this.scrollX,
        scrollY: this.scrollY,
        momentum: false,
        snap: {
          loop: this.loop,
          threshold: 0.3,
          speed: 400
        },
        click: this.click,
        bounce: this.bounce
      })
      // 利用轮播原理左右翻页
      this.slider.on('scrollEnd', () => {
        let pageIndex = this.slider.getCurrentPage().pageX
        console.log(pageIndex)
        if (this.loop) {
          pageIndex -= 1
        }
        this.currentPageIndex = pageIndex
        // 将当前页码传给父组件
        this.$emit('switchTab', this.currentPageIndex)
        if (this.autoPlay) {
          clearTimeout(this.timer)
          this._play()
        }
      })
    },
    _play () {
      let pageIndex = this.currentPageIndex + 1
      if (this.loop) {
        pageIndex += 1
      }
      this.timer = setTimeout(() => {
        this.slider.goToPage(pageIndex, 0, 400)
      }, this.interval)
    },
    destroyed () {
      clearTimeout(this.timer) // 当组件中用到timer时，在销毁时要清除
    }
  }
}
</script>

<style lang="less" scoped>
.wrapper{
  min-height: 1px;
  .content{
    position: relative;
    overflow: hidden;
    white-space: nowrap;
    min-height: 100%;
    .slider-item{
      float: left;
      box-sizing: border-box;
      overflow: hidden;
      text-align: center;
      &::after{
        content: '';
        display: block;
        clear: both;
      }
      // 这里的样式单纯为了能看出各个页面切换了
      &:nth-child(1) {
        color: red;
      }
      &:nth-child(2) {
        color: #000;
      }
      &:nth-child(3) {
        color: #fff;
      }
      &:nth-child(4) {
        color: yellow;
      }
      &:nth-child(5) {
        color: blue;
      }
    }
  }
  .dots{
      position: absolute;
      right: 0;
      left: 0;
      bottom: 12px;
      text-align: center;
      font-size: 0;
      .dot{
        display: inline-block;
        margin: 0 4px;
        width: 8px;
        height: 8px;
        border-radius: 50%;
        background: #eee;

        &.active{
          width: 20px;
          border-radius: 5px;
          background: #aaa;
        }
      }
    }
  }
</style>
