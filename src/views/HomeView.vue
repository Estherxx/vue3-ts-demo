<template>
  <div class="home">
    <el-container>
      <el-header height="80px">
        <el-row :gutter="20">
          <el-col :span="4"
            ><img src="../assets/logo.png" class="logo" />
            <div class="grid-content ep-bg-purple"
          /></el-col>
          <el-col :span="16"
            ><h2>后台管理系统</h2>
            <div class="grid-content ep-bg-purple"
          /></el-col>
          <el-col :span="4">
            <el-button style="margin-top: 25px" @click="delToken"
              >退出登录</el-button
            ></el-col
          >
        </el-row></el-header
      >
      <el-container>
        <el-aside width="200px">
          <el-menu
            active-text-color="#ffd04b"
            background-color="#545c64"
            class="el-menu-vertical-demo"
            :default-active="active"
            text-color="#fff"
            router
          >
            <el-menu-item
              :index="item.path"
              v-for="item in list"
              :key="item.path"
            >
              <span>{{ item.meta.title }}</span>
            </el-menu-item>
          </el-menu>
        </el-aside>
        <el-main><router-view /></el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useRouter, useRoute } from "vue-router";
export default defineComponent({
  name: "HomeView",
  setup() {
    const router = useRouter();
    const route = useRoute();
    //console.log(router.getRoutes());
    const list = router.getRoutes().filter((v) => v.meta.isShow);
    const delToken = () => {
      localStorage.removeItem("token");
      router.push("/login");
    };
    //console.log(list);
    return { list, active: route.path, delToken };
  },
  components: {},
});
</script>
<style lang="scss" scoped>
.logo {
  height: 80px;
}
.el-header {
  height: 80px;
  background-color: rgb(81, 78, 78);
  h2,
  .quit-login {
    text-align: center;
    color: aliceblue;
    line-height: 80px;
  }
}
.el-aside {
  .el-menu {
    height: 650px;
  }
}
</style>
