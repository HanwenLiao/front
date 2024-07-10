<template>
  <el-container style="height: 100vh;">
    <el-header>
      <h1>SDK Center</h1>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu :default-active="activeMenu" @select="handleMenuSelect">
          <el-menu-item index="home">HOME</el-menu-item>
          <el-menu-item index="uploadSdk">上传sdk</el-menu-item>
          <el-menu-item index="detectTask">检测任务</el-menu-item>
          <el-menu-item index="sdkList">sdk列表</el-menu-item>
          <el-menu-item index="permissionSettings">权限设置</el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <el-main>
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </el-container>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'App',
  setup() {
    const router = useRouter();
    const activeMenu = ref('home');

    const handleMenuSelect = (index: string) => {
      if (index === 'home') {
        router.push('/');
      } else if (index === 'uploadSdk') {
        router.push('/upload-sdk');
      } else if (index === 'detectTask') {
        router.push('/detect-tasks');
      } else if (index === 'sdkList') {
        router.push('/sdk-list');
      } else if (index === 'permissionSettings') {
        router.push('/permission-settings');
      }
      activeMenu.value = index;
    };

    return {
      activeMenu,
      handleMenuSelect,
    };
  },
});
</script>

<style>
.el-container {
  width: 200%;
  height: 100%;
}

.el-header {
  background-color: #b3c0d1;
  color: #333;
  line-height: 60px;
  text-align: center;
  font-size: 24px;
}

.el-aside {
  background-color: #d3dce6;
}

.el-main {
  background-color: #e9eef3;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
  width: 100%;
  overflow: auto;
}
</style>