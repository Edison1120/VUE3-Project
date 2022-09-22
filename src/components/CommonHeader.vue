<template>
  <el-header>
    <div class="l-content">
      <!-- 图标展示 -->
      <el-button size="small" plain @click="handleCollapse">
        <el-icon :size="20">
          <Menu />
        </el-icon>
      </el-button>
      <el-breadcrumb separator="/" class="bread">
        <!-- 首页是一定存在的 所以直接写死 -->
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item :to="current.path" v-if="current">{{
          current.label
        }}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="r-content">
      <el-dropdown>
        <span class="el-dropdown-link">
          <img class="user" src="../assets/user1.jpg" alt="" />
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item>个人中心</el-dropdown-item>
            <el-dropdown-item @click="handleLoginOut">退出</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </el-header>
</template>
<script>
import { computed } from "@vue/runtime-core";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
export default {
  setup() {
    let store = useStore();
    // const imgSrc = require("../assets/user1.jpg");老方法
    // 不用箭头函数试试看能不能调用getImgSrc
    const getImgSrc = () => {
      return new URL("../assets/user1.jpg", import.meta.url).herf;
    };
    let handleCollapse = () => {
      // 调用VUEX中的mutatinos
      store.commit("updatedIsCollapse");
    };
    //计算属性
    const current = computed(() => {
      return store.state.currentMenu;
    });
    const router = useRouter();
    const handleLoginOut = () => {
      store.commit("cleanMenu");
      router.push({
        name: "login",
      });
    };
    return {
      // userImg,
      getImgSrc,
      handleCollapse,
      current,
      handleLoginOut,
    };
  },
};
</script>
<style lang="less" scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  background: #333;
}
.r-content {
  .user {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }
}
.l-content {
  display: flex;
  align-items: center;
  .el-button {
    margin-right: 20px;
  }
  h3 {
    color: #fff;
  }
}
.bread /deep/ span {
  color: #fff !important;
  cursor: pointer !important;
}
</style>

