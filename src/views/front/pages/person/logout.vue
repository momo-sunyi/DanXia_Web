<template>
  <v-img
    class="fill-height"
    src="../../../../assets/新登录背景.png"
    :aspect-ratio="16 / 8.4"
    height="100vh"
    width="100vw"
  >
    <!-- 添加黑色透明度40%的蒙版 -->
    <div style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.6);"></div>

    <v-container fill-height fluid>
      <!-- 调整卡片位置，使其下移 -->
      <v-row justify="center" align="center" class="fill-height" style="margin-top: 250px;">
        <v-col cols="12" md="8" lg="6" xl="4">
          <!-- 调整卡片样式与登录页面一致 -->
          <v-card
            elevation="12"
            class="py-8"
            style="border-radius: 20px; background-color: rgba(255, 255, 255, 0.15); backdrop-filter: blur(3px);"
          >
            <v-card-text class="text-center">
              <!-- 修改标题字体颜色为白色，并强制生效 -->
              <h2 class="text-h4 font-weight-bold" style="color: white !important;">确认退出登录</h2>

              <div class="mt-6">
                <v-row justify="center">
                  <v-col cols="12" md="10">
                    <div class="mt-6">
                      <!-- 修改确认按钮字体颜色为黑色 -->
                      <v-btn
                        block
                        class="text-capitalize"
                        large
                        color="primary"
                        @click.stop="logout"
                        style="border-radius: 20px; color: black !important;"
                      >确认</v-btn>
                      <!-- 增加按钮之间的间距 -->
                      <p style="margin-top: 20px;"></p>
                      <!-- 修改取消按钮字体颜色为黑色 -->
                      <v-btn
                        block
                        class="text-capitalize"
                        large
                        color="information"
                        @click="router.back()"
                        style="border-radius: 20px; color: black !important;"
                      >取消</v-btn>
                    </div>
                  </v-col>
                </v-row>
              </div>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-img>
</template>

<script>
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';

export default {
  name: "Login",
  setup() {
    const store = useStore();
    const router = useRouter();
    
    const logout = () => {
      // 将用户信息清空
      store.commit("loginIn", null);
      router.push("/Login"); // 跳转登录页面
    };
    
    return {
      router,
      logout
    };
  }
};
</script>