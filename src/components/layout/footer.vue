<template>
  <div>
    <v-footer color="primary" dark app absolute class="py-0 ma-0" height="auto" style="margin: 0; padding: 0;">
      <v-container fluid class="pa-0">  <!-- 修改为 fluid 并移除所有内边距 -->
        <v-row align="center" class="my-0">
          <v-col cols="12" md="10" class="py-0">
            <v-row align="center">
              <!-- 添加图标 -->
              <v-col cols="auto" style="padding-left: 130px !important; margin-top: 20px;">  
                <v-img src="../../assets/图标.png" width="200" height="200"></v-img>
              </v-col>

              <!-- 地址信息靠左对齐 -->
              <v-col class="text-left" style="padding-left: 100px !important;">  <!-- 使用内联样式强制生效 -->
                <h4 class="text-h5 font-weight-bold mb-1" style="color: green !important; margin-bottom: 0 !important; margin-top: 30px !important;">华南农业大学</h4>  <!-- 上移 -->
                <p class="text-caption" style="margin-top: 30px !important; font-size: 14px !important; line-height: 2.1;">  <!-- 增大字体和行间距 -->
                  广东省广州市天河区五山路483号华南农业大学<br />
                  邮编： 510642<br />
                  微信公众号： 虚拟丹霞pro<br />
                </p>
              </v-col>
            </v-row>
          </v-col>
         
          <!-- 右侧往年回顾 -->
          <v-col cols="12" md="2" class="footer-section" style="margin-left: -250px; margin-top: 60px;  margin-left: -300px !important ; flex-grow: 1; "> <!-- 添加 background-color: transparent -->
            <v-list flat color="transparent" class="historical-records-section" style="background-color: rgba(255, 255, 255, 0.2); width: 350px !important; "> <!-- 设置背景为透明 -->
              <v-list-subheader class="text-h6 history-title" style="color: green !important; border-bottom: 2px solid green; background-color: transparent;">往年回顾</v-list-subheader>
              <div class="video-buttons d-flex" style="gap: 10px;">
                <v-btn
                  color="white"
                  variant="outlined"
                  prepend-icon="mdi-video"
                  class="video-btn"
                  @click="checkVideoFun"
                >
                  <div class="btn-content">
                    <span class="btn-title">2021级生态学专业</span>
                    <small style="margin-left: 10px; margin-top: 5px;">自然地理学野外综合实习</small>
                  </div>
                </v-btn>
                
                <v-btn
                  color="white"
                  variant="outlined"
                  prepend-icon="mdi-video"
                  class="video-btn"
                  @click="checkVideoFun1"
                >
                  <div class="btn-content">
                    <span class="btn-title">2022级地理信息科学</span>
                    <small>专业地理综合实习</small>
                  </div>
                </v-btn>
              </div>
            </v-list>
          </v-col>
        </v-row>

        <div class="text-center mt-6 footer-bottom" style="background-color: #006400; padding: 10px;">
          Copyright &copy;
          <a
            class="white--text"
            href="*"
            target="_blank"
            rel="noopener noreferrer"
            >虚拟丹霞</a
          >
          {{ new Date().getFullYear() }}. All rights reserved.
        </div>
        
      </v-container>
    </v-footer>
    <template>
      <div>
        <!-- 视频播放模态框 -->
        <v-dialog v-model="videoState" max-width="800" transition="dialog-bottom-transition">
          <v-card style="background-color: rgba(255, 255, 255, 0.2);">
            <v-card-text>
              <video :src="videoSrc" controls autoplay style="width: 100%;"></video>
            </v-card-text>
            <v-card-actions>
              <v-btn color="white" @click="videoState = false" style="border-radius: 20px; background-color: rgba(0, 100, 0, 0.9);">关闭</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
    
        <v-dialog v-model="videoState1" max-width="800" transition="dialog-bottom-transition">
          <v-card style="background-color: rgba(255, 255, 255, 0.2);">
            <v-card-text>
              <video :src="videoSrc1" controls autoplay style="width: 100%;"></video>
            </v-card-text>
            <v-card-actions>
              <v-btn color="white" @click="videoState1 = false" style="border-radius: 20px; background-color: rgba(0, 100, 0, 0.9);">关闭</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </template>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const videoSrc = ref("video/生态实习视频.mp4");
    const videoSrc1 = ref("video/实习视频.mp4");
    const videoState = ref(false);
    const videoState1 = ref(false);
    
    const checkVideoFun = () => {
      videoState.value = true;
    };

    const checkVideoFun1 = () => {
      videoState1.value = true;
    };

    return {
      videoSrc,
      videoSrc1,
      videoState,
      videoState1,
      checkVideoFun,
      checkVideoFun1
    };
  }
};
</script>

<style scoped>
.mask {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 10;
  background-color: #000000;
  opacity: 0.6;
}
/* 内容层 z-index要比遮罩大，否则会被遮盖 */
.videomasks {
  max-width: 80%;
  width: 800px;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 20;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
}

/* 修改往年回归区域的背景颜色 - 与主题一致 */
.historical-records-section {
  background-color: var(--v-primary-base, #1976d2); /* 使用与主题相同的颜色 */
  border-radius: 8px;
  padding: 15px;
  margin: 10px 0;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

/* 往年回顾标题样式 */
.history-title {
  font-weight: bold;
  color: white !important;
  border-bottom: 2px solid rgba(255, 255, 255, 0.3);
  margin-bottom: 12px;
  padding-bottom: 8px;
}

/* 视频按钮容器 */
.video-buttons {
  padding: 5px 0;
  white-space: nowrap; /* 禁止换行 */
  width: 100%; /* 确保容器宽度 */
}

/* 视频按钮滚动动画 */
@keyframes scrollButtons {
  0% {
    transform: translateX(10%); /* 从右侧开始 */
  }
  100% {
    transform: translateX(-30%); /* 滚动到左侧 */
  }
}

.video-buttons {
  animation: scrollButtons 4s linear infinite; 
}

/* 视频按钮样式优化 */
.video-btn {
  text-align: left;
  border-radius: 6px !important;
  padding: 8px 10px !important;
  transition: all 0.3s ease;
  height: auto !important;
  min-height: 60px;
  line-height: 1.2;
  border: 1px solid white !important;
  color: white !important;
  background-color: rgba(255, 255, 255, 0.1) !important;
  display: inline-block; /* 确保按钮水平排列 */
  margin-right: 10px; /* 按钮间距 */
}

.video-btn .v-icon {
  color: white !important;
  margin-right: 5px;
}

.btn-content {
  display: flex;
  flex-direction: column;
  width: 100%;
  overflow: hidden;
}

.btn-title {
  font-weight: 500;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}

.video-btn small {
  display: block;
  opacity: 0.85;
  font-size: 0.75em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}

.video-btn:hover {
  background-color: rgba(255, 255, 255, 0.2) !important;
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* 视频模态框优化 */
.videomasks {
  max-width: 80%;
  width: 800px;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 20;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
}

/* 修复往年回归区域的背景颜色 */
.historical-records-section {
  background-color: #f5f5f5; /* 浅灰色背景，替代纯白色 */
  border-radius: 8px;
  padding: 20px;
  margin: 15px 0;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

/* 往年回顾标题样式 */
.history-title {
  font-weight: bold;
  color: #1976d2 !important;
  border-bottom: 2px solid #1976d2;
  margin-bottom: 16px;
  padding-bottom: 8px;
}

/* 视频按钮容器 */
.video-buttons {
  padding: 8px 0;
}

/* 视频按钮样式 */
.video-btn {
  text-align: left;
  border-radius: 8px !important;
  padding: 8px 16px !important;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  height: auto !important;
  line-height: 1.3;
}

.video-btn small {
  display: block;
  opacity: 0.85;
  font-size: 0.8em;
  margin-top: 2px;
}

.video-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* 美化整体 footer */
.footer {
  background-color: var(--v-primary-base, #1976d2);
  color: white;
  padding: 30px 0 20px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
}

.footer-section {
  margin-bottom: 20px;
}

.footer-title {
  font-weight: 500;
  margin-bottom: 15px;
  border-bottom: 2px solid rgba(255, 255, 255, 0.2);
  padding-bottom: 8px;
}

.footer-links a,
.footer-links div {
  transition: color 0.3s;
  display: block;
  padding: 5px 0;
}

.footer-links a:hover {
  color: #b3e5fc !important;
}

.footer-bottom {
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding-top: 15px;
  margin-top: 10px;
  text-align: center;
  font-size: 0.85rem;
}

.social-icons {
  display: flex;
  gap: 15px;
  margin-top: 10px;
}

/* 适配移动端 */
@media (max-width: 960px) {
  .footer {
    padding: 20px 0 10px;
  }
  
  .videomasks {
    width: 95%;
  }
}
</style>
