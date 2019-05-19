<template>
  <div class="main">
    <!-- 热门话题 -->
    <div class="single-application bra">
      <div class="single-application-title">
        <text class="f28 fw5 c0">单应用入口</text>
      </div>
      <div class="single-application-content flex-ac">
        <bui-image src="/image/test.jpg" radius='8px' width="654px" height="212px"></bui-image>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        singleApplicationArr: ['', '', '', '']
      }
    },
    methods: {
      // 更多
      singleApplicationMoreEvent() {
        this.$alert()
      },
      // 常用联系人
      singleApplicationUserEvent() {
        this.$alert()
      }
    }
  }
</script>

<style lang="css" src="../css/common.css"></style>
<style>
  .main {
    flex: 1;
    padding-top: 100px;
    background-color: #666;
  }

  .single-application {
    background-color: #fff;
  }

  .single-application-title {
    height: 40px;
    margin: 18px 23px 20px 25px;
  }
  .single-application-content {
    margin-bottom: 36px;
  }
</style>