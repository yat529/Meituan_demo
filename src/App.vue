<template>
  <div id="app">
    <!-- Header Section -->
    <!-- 向AppHeader组件传递poi_info对象 -->
    <AppHeader :poi="poi_info"></AppHeader>
    <!-- Navi Section -->
    <AppNav :commentCount="commentCount"></AppNav>
    <!-- Main Content Section -->
    <keep-alive>
      <router-view class="main"></router-view>
    </keep-alive>
  </div>
</template>

<script>
// 导入 AppHeader组件
import AppHeader from "@/components/header/header";
// 导入 AppNav组件
import AppNav from "@/components/nav/nav";

export default {
  name: "App",
  // 本地注册子组件
  components: {
    AppHeader,
    AppNav
  },
  // 声明本地属性
  data() {
    return {
      poi_info: {},
      commentCount: 0,
    };
  },

  // 在create钩子处使用$axios后台加载数据
  // 加载的数据存入poi_info对象,方便AppHeader组件读取
  created() {
    // save this to that
    let that = this;

    // 后台加载数据
    that.$axios
      .get("/api/order")
      .then(response => {
        let sourceData = response.data;

        if (sourceData.code == 0) {
          that.poi_info = sourceData.data.poi_info;
        }
      })
      .catch(error => {
        console.log(error);
      });

    // 后台加载数据
    that.$axios
      .get("/api/rating")
      .then(response => {
        let sourceData = response.data;

        if (sourceData.code == 0) {
          that.commentCount = sourceData.data.comment_num;
        }
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style lang="scss">
// 导入字体图标
@import url("./common/css/icon.css");

.main {
  position: absolute;
  width: 100%;
  top: 201px;
  left: 0;
  bottom: 51px;
  overflow: hidden;
}
</style>