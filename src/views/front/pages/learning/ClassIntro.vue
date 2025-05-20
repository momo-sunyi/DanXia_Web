<template>
  <v-app style="background-color: #C8E6C9;"> <!-- 修改为绿色背景 -->
    <v-container fluid>
      <!-- 顶部标签页导航 -->
      <div class="tab-container">
        <router-link :to="{ name: 'ClassIntro', params: { currentClass: currentClass } }" class="tab-item"
          :class="{ 'active': model === 0 }" @click="model = 0">
          <v-icon left>mdi-pen</v-icon>
          课程介绍
        </router-link>
        <router-link :to="{ name: 'ClassList', params: { currentClass: currentClass } }" class="tab-item"
          :class="{ 'active': model === 1 }" @click="model = 1">
          <v-icon left>mdi-book</v-icon>
          课程内容
        </router-link>
        <div class="tab-slider" :style="sliderStyle"></div>
      </div>

      <!-- 修改容器样式 -->
      <v-container fluid style="padding: 0 0%;"> <!-- 添加左右padding控制 -->
        <!-- ... 顶部导航保持不变 ... -->

        <!-- 修改主内容区域 -->
        <v-container fluid style="padding: 0px 50px 0 12px;"> <!-- 添加适当padding -->
          <!-- 顶部标签页导航保持不变... -->

          <!-- 主内容区域 -->
          <div id="r">
            <v-card class="mx-auto" min-height="100%" width="100%" flat>
              <v-card-text class="content-card">
                <!-- 课程信息区域 -->
                <!-- 在课程信息区域下方添加导航栏 -->
                <div class="info-image-container">
                  <div class="info-block" style="padding-top:-15px; ">
                    <div style="display: flex; align-items: center; justify-content: space-between; width: 100%;">
                      <p class="text-h4 font-weight-bold" style="margin-left: -10px;">{{ currentItem.title }}</p>
                      <v-btn 
                        v-if="currentItem.title"
                        @click="openMoocLink(currentItem.title)"
                        color="primary"
                        class="ml-2"
                        small
                        outlined
                        style="margin-right:170px;"  
                      >
                        <v-icon left small>mdi-open-in-new</v-icon>
                        点击进入中国MOOC学习
                      </v-btn>
                    </div>
                    <p class="text-h5 mt-3" style="margin-left: -10px;">{{ currentItem.teacher }}</p>
                    <!-- 修改课程性质区域 -->
                    <div class="course-property mt-4" style="margin-left: -25px;"> <!-- 增加左移距离 -->
                      <p class="text-h6 font-weight-bold mb-2">课程性质：</p>
                      <v-textarea v-if="isTeacher" v-model="currentItem.xingzhi" @change="updateIntro" auto-grow
                        class="custom-textarea" style="font-size: 16px;"></v-textarea>

                      <p v-else class="course-property-content" style="font-size: 13px;">
                        {{ currentItem.xingzhi }}
                      </p>
                    </div>
                  </div>

                  <v-btn class="back-button" @click="goBack" icon color="primary">
                    <v-icon>mdi-arrow-left</v-icon>
                  </v-btn>

                  <!-- 修改图片容器样式 -->
                  <v-img :src="courseImage" contain max-height="300" class="course-image"
                    style="max-width: 35%; margin-top: 105px;"></v-img>
                </div>

                <!-- 添加导航栏 -->
                <div class="content-nav">
                  <v-btn text @click="activeTab = 'aim'" :class="{ 'active-tab': activeTab === 'aim' }">
                    课程目的与要求
                  </v-btn>
                  <v-btn text @click="activeTab = 'difficult'" :class="{ 'active-tab': activeTab === 'difficult' }">
                    教学重点与难点
                  </v-btn>
                  <v-btn text @click="activeTab = 'content'" :class="{ 'active-tab': activeTab === 'content' }">
                    教学内容
                  </v-btn>
                </div>

                <!-- 条件渲染的内容区域保持不变 -->
                <template v-if="activeTab === 'aim'">
                <div class="content-block" :class="{ 'active-content': activeTab === 'aim' }">
                <p class="ml-5 text-h6 font-weight-bold">课程目的与要求 ：</p>
                <v-textarea class="ml-5" v-if="isTeacher" v-model="currentItem.aim" @change="updateIntro" auto-grow
                  style="font-family: '楷体', sans-serif; font-size: 18px;"></v-textarea>
                <p class="mx-8" style="white-space: pre-wrap" v-if="!isTeacher">
                  {{ currentItem.aim }}
                </p>
              </div>
              </template>

              <template v-if="activeTab === 'difficult'">
                <div class="content-block" :class="{ 'active-content': activeTab === 'difficult' }">
                <p class="ml-5 text-h6 font-weight-bold">教学重点与难点 ：</p>
                <v-textarea class="ml-5" v-if="isTeacher" v-model="currentItem.difficult" @change="updateIntro"
                  auto-grow style="font-family: '楷体', sans-serif; font-size: 18px;"></v-textarea>
                <p class="mx-8" style="white-space: pre-wrap" v-if="!isTeacher">
                  {{ currentItem.difficult }}
                </p>
              </div>
              </template>

              <template v-if="activeTab === 'content'">
                <div class="content-block" :class="{ 'active-content': activeTab === 'content' }">
                <p class="ml-5 text-h6 font-weight-bold">教学内容 ：</p>
                <v-textarea class="ml-5" v-if="isTeacher" v-model="currentItem.content" @change="updateIntro"
                  auto-grow></v-textarea>
                <p class="mx-8" style="white-space: pre-wrap" v-if="!isTeacher">
                  {{ currentItem.content }}
                </p>
              </div>
              </template>
              </v-card-text>
            </v-card>
          </div>
        </v-container>
      </v-container>
    </v-container>

  </v-app>
</template>



<script>
import { ref, reactive, onMounted, computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from "axios";

export default {
  name: "ClassIntro",
  
  setup() {
    const route = useRoute();
    const router = useRouter();
    const currentClass = ref("自然地理"); // 设置默认值
    const isTeacher = ref(sessionStorage.getItem("isteacher") == "true" ? true : false);
    const reveal = ref(false);
    const model = ref(0);
    
    const currentItem = ref({});
    const InfoItem = reactive([]);
    
    const updateIntro = () => {
      axios({
        url: "https://danxiagis.top:8081/teacher/updateCourseIntro",
        method: "post",
        data: {
          couName: currentItem.value.title,
          teaName: currentItem.value.teacher,
          nature: currentItem.value.xingzhi,
          objectives: currentItem.value.aim,
          focus: currentItem.value.difficult,
          content: currentItem.value.content,
        },
      }).then((res) => {
        console.log(res);
        alert(res.data);
        CurrentItem();
      });
    };
    
    const goBack = () => {
      router.push({
        name: "FirstView",
      });
    };
    
    const activeTab = ref('aim'); 

    const CurrentItem = () => {
      axios({
        url: "https://danxiagis.top:8081/teacher/courseIntro/get",
        method: "get",
      }).then((res) => {
        Object.assign(InfoItem, res.data);
        for (let i = 0; i < InfoItem.length; i++) {
          if (InfoItem[i].courseName == currentClass.value) {
            currentItem.value = {
              title: InfoItem[i].courseName,
              xingzhi: InfoItem[i].natureCurriculum,
              teacher: InfoItem[i].teacherName,
              content: InfoItem[i].teaching_content,
              aim: InfoItem[i].course_objectives,
              difficult: InfoItem[i].teaching_focus,
            };
          }
        }
      });
    };
    
    const PassClass = () => {
      currentClass.value = route.params.currentClass;
    };
    
    onMounted(() => {
      PassClass();
      CurrentItem();
    });
    
    const sliderStyle = computed(() => {
      const tabWidth = 100 / 2; // 两个标签平分宽度
      return {
        width: `${tabWidth}%`,
        transform: `translateX(${model.value * 100}%)`
      };
    });

    const courseImages = {
      "自然地理": require('@/assets/自然地理.png'),
      "人文与经济地理": require('@/assets/人文地理.png'),
      "地图学": require('@/assets/地图学.png'),
      "地理信息系统原理": require('@/assets/地理信息系统.png'),
      // 添加更多课程图片映射
    };

    const courseImage = computed(() => {
      return courseImages[currentClass.value] || require('@/assets/丹霞山1.png');
    });

    const openExternalLink = (url) => {
      window.open(url, '_blank');
    };

    const moocLinks = {
          "自然地理": "https://www.icourse163.org/search.htm?search=%E8%87%AA%E7%84%B6%E5%9C%B0%E7%90%86#/",
          "人文与经济地理": "https://www.icourse163.org/search.htm?search=%E4%BA%BA%E6%96%87%E5%9C%B0%E7%90%86#/",
          "地图学": "https://www.icourse163.org/search.htm?search=%E5%9C%B0%E5%9B%BE%E5%AD%A6#/",
          "地理信息系统原理": "https://www.icourse163.org/search.htm?search=%E5%9C%B0%E7%90%86%E4%BF%A1%E6%81%AF%E7%B3%BB%E7%BB%9F#/"
        };
    
        const openMoocLink = (courseName) => {
          const url = moocLinks[courseName] || "https://www.icourse163.org/";
          window.open(url, '_blank');
        };
    
    return {
      isTeacher,
      reveal,
      model,
      currentClass,
      currentItem,
      InfoItem,
      updateIntro,
      goBack,
      CurrentItem,
      PassClass,
      sliderStyle,
      courseImage,
      activeTab,
      openExternalLink,
      openMoocLink // 确保这个方法被返回
    };
  }
};
</script>

<style>
.tab-container {
  position: relative;
  display: flex;
  background: #f5f5f5;
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 20px;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 12px 16px;
  position: relative;
  color: #333;
  text-decoration: none;
  transition: all 0.3s ease;
  z-index: 1;
}

.tab-item.active {
  color: #4CAF50;
  font-weight: bold;
}

.tab-slider {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 3px;
  background: #4CAF50;
  border-radius: 3px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 2;
}


/* 移除主内容区域阴影 */
#r {
  min-height: 900px;
  margin: 0 auto 40px;
  width: 100%;
  padding: 0;

  min-width: 100%;
  position: relative;
  margin-bottom: 40px;
  border-radius: 0;
  box-shadow: none !important; /* 移除阴影 */
}

/* 移除课程性质区域的阴影 */
.course-property {
  background: rgba(255, 255, 255, 0.7);
  padding: 15px;
  border-radius: 0; /* 移除圆角 */
  box-shadow: none !important; /* 移除阴影 */
}

/* 移除图片阴影 */
.course-image {
  flex: 1;
  max-width: 40%;
  border-radius: 0;
  box-shadow: none !important; /* 移除阴影 */
  border: none !important; /* 移除边框 */
}

.info-image-container {
  display: flex;
  align-items: stretch;
  margin: 20px 0;
}

.info-block {
  flex: 1;
  background: #e8f5e9; /* 浅绿色背景 */
  padding: 30px;
  border-radius: 8px 0 0 8px;
}

.course-image {
  flex: 1;
  max-width: 40%;
  border-radius: 0 8px 8px 0;
}

/* 修改课程性质区域样式 */
.course-property {
  background: rgba(0,0,0,0.05) !important; /* 浅灰色蒙版 */
  padding: 18px;
  border-radius: 4px;
  box-shadow: none !important;
}

.course-property-content {
  background: rgba(255,255,255,0.8) !important; /* 白色半透明背景 */
  padding: 5px;
  border-radius: 4px;
}

.custom-textarea {
  background: rgba(58, 44, 44, 0.8) !important; /* 白色半透明背景 */
}

.course-property-content {
  font-family: inherit !important; /* 使用系统字体 */
  font-size: 16px;
  line-height: 1.6;
  color: #333;
  margin-top: 8px;
}

/* 添加全局字体设置 */
body, .v-application {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif !important;
}

/* 修改卡片样式 */
.content-card {
  background: rgba(255, 255, 255, 0.85) !important;
  border-radius: 0 !important;
  padding: 24px;
}

/* 调整信息块背景色 */
.info-block {
  background: rgba(255, 255, 255, 0.5) !important;
  padding: 20px 30px; /* 调整padding */
  border-radius: 0;
}

/* 修改文本区域字体 */
.custom-textarea {
  font-family: inherit !important; /* 使用系统字体 */
  font-size: 16px;
  background: #fff;
  border-radius: 6px;
  padding: 8px;
}

.course-property-content {
  font-family: inherit !important; /* 使用系统字体 */
  font-size: 16px;
  line-height: 1.6;
  color: #333;
  margin-top: 8px;
}

.back-button {
  position: absolute;
  right: 20px;
  top:90px;
  transform: translateY(-50%);
}

/* 调整主内容区域样式 */
#r {
  min-height: 900px;
  width: 100%;
  margin: 0;
  padding: 0;
}

/* 确保所有元素使用border-box模型 */
* {
  box-sizing: border-box;
}

.content-nav {
  display: flex;
  margin: 20px 0;
  padding: 0;
  border: none;
}

.content-nav .v-btn {
  margin-right: 15px;
  text-transform: none;
  font-size: 16px;
}

.content-nav .active-tab {
  color: #4CAF50;
  font-weight: bold;
  border-bottom: 2px solid #4CAF50;
}

.content-nav .nav-btn {
  flex: 1;
  margin: 0;
  border-radius: 0;
  border-right: 1px solid #e0e0e0;
}

.content-nav .nav-btn:last-child {
  border-right: none;
}

.content-nav .active-tab {
  background-color: #4CAF50;
  color: white !important;
}

/* 添加内容区域样式 */
.content-block {
  padding: 15px;
  border-radius: 0 0 4px 4px;
}

.active-content {
  background-color: rgba(76, 175, 80, 0.1);
  border-left: 3px solid #4CAF50;
}

</style>