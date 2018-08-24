<template>
  <!-- 搜索栏 -->
  <div class="search">
    <div class="search_container">
      <div class="ewm flex1">
        <span class="icon"><i class="iconfont icon-saoyisao"></i></span>
          <p class="search_tl">扫一扫</p>
      </div>
      <div class="search_form flex5">
        <div class="search_form_btn">
          <input type="text" placeholder='搜索商品，共122497款好物'>
        </div>
      </div>
      <div class="notice flex1">
        <span class="icon"><i class="iconfont icon-xiaoxi"></i></span>
        <p class="search_tl">消息</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      goods: 122497
    }
  },
  methods: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.flex1{
  flex: 1;
  .search_tl{
    color: #aaa;
    font-size: 12px;
  }
}
.flex2{
  flex: 2;
}
.flex3{
  flex: 3;
}
.flex4{
  flex: 4;
}
.flex5{
  flex: 5;
}
.search{
  width: 100%;
  &_container{
    display: flex;
    padding: 0.2rem;
    justify-content: center;
    align-items: center;

    .ewm{
      font-size: 12px;
      span{
        display: block;
        width: 20px;
        height: 20px;
        font-size: 14px;
        margin: 0 auto;
      }
    }
    .search_form{
      height: auto;
      background: #eee;
      border-radius: 4px;
      &_btn{
        padding: 0 1rem;
        height: 100%;
        display: block;
        input{
          display: block;
          width: 100%;
          border: none;
          padding: 0.4rem 0;
          background: transparent;
          padding-left: 1rem;
        }
      }
    }

    .notice{
      font-size: 14px;
    }
  }
}
</style>
