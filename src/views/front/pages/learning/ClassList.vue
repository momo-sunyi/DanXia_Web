<template>
  <v-app style="background-color: #C8E6C9;">
    <v-container fluid>
      <!-- 顶部标签页导航 -->
      <div class="tab-container">
        <v-btn 
          @click="$router.push({ name: 'FirstView' })" 
          icon
          class="back-button"
          color="primary"
          style="position: absolute; right: 80px; top: 196px;"
        >
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
        <router-link 
          :to="{ name: 'ClassIntro', params: { currentClass: currentClass } }" 
          class="tab-item"
          :class="{ 'active': model === 0 }" 
          @click="model = 0">
          <v-icon left>mdi-pen</v-icon>
          课程介绍
        </router-link>
        <router-link 
          :to="{ name: 'ClassList', params: { currentClass: currentClass } }" 
          class="tab-item"
          :class="{ 'active': model === 1 }" 
          @click="model = 1">
          <v-icon left>mdi-book</v-icon>
          课程内容
        </router-link>
        <div class="tab-slider" :style="sliderStyle"></div>
      </div>

      <!-- PDF查看器模态框 -->
      <v-dialog v-model="selectedPdf" max-width="90%" persistent>
        <v-card>
          <v-card-title>
            <v-btn icon @click="closePdf">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text>
            <iframe 
              :src="selectedPdf" 
              width="100%" 
              height="600px"
              style="border: none;"
            ></iframe>
          </v-card-text>
        </v-card>
      </v-dialog>

      <!-- 课程内容区域 -->
      <v-card class="mx-auto" style="margin: 40px 2% 0; width: 95%;">
        <v-card-title class="text-h4 font-weight-bold" style="margin: 50px 0 0 27px;"> 
          {{ currentClass }}课程内容
        </v-card-title>
        
        <v-card-text>
          <!-- 列表容器 -->
          <div class="list-container">
            <v-list class="py-0" style="width: 99%; margin: 0 auto;">
              <v-list-group 
                v-for="(chapter, index) in filteredChapters" 
                :key="index"
                v-model="chapter.active"  
                prepend-icon="mdi-book-open"
                class="chapter-item"
              >
                <template v-slot:activator="{ props }">
                  <v-list-item v-bind="props" class="chapter-header">
                    <v-list-item-title class="chapter-title">{{ chapter._id }}</v-list-item-title>
                  </v-list-item>
                </template>

                <v-list-item
                  v-for="(item, i) in chapter.info"
                  :key="i"
                  @click="openPdf(item.pdf)"
                  class="topic-item"
                >
                  <v-list-item-title class="topic-title">{{ item.topic }}</v-list-item-title>
                  <v-list-item-icon>
                    <v-icon color="primary">mdi-file-pdf-box</v-icon>
                  </v-list-item-icon>
                </v-list-item>
              </v-list-group>
            </v-list>
          </div>
        </v-card-text>
      </v-card>

      <!-- PDF查看器模态框 -->
      <v-dialog v-model="selectedPdf" max-width="90%" persistent>
        <v-card>
          <v-card-title>
            <v-btn icon @click="closePdf">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text>
            <iframe 
              :src="selectedPdf" 
              width="100%" 
              height="600px"
              style="border: none;"
            ></iframe>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-container>
  </v-app>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

export default {
  name: "ClassList",
  setup() {
    const route = useRoute();
    const currentClass = ref(route.params.currentClass || "地理信息系统原理");
    const model = ref(1);
    const chapters = ref([]);
    const selectedPdf = ref(null); // 添加选中的PDF状态

    // 获取课程目录
    const loadClassList = async () => {
      try {
        const res = await axios.get('https://danxiagis.top:8081/uploadData/get', {
          params: { courseName: currentClass.value }
        });
        
        // 处理章节名，去前缀
        const processedData = res.data.map(item => {
          const chapterName = item._id.split('—')[1] || item._id;
          return {
            ...item,
            _id: chapterName,
            active: false,  // 添加active状态初始化为false
            info: item.info.map(infoItem => ({
              ...infoItem,
              chapter: chapterName
            }))
          };
        });

        // 按章节数字排序
        processedData.sort((a, b) => {
          // 提取章节数字
          const getChapterNum = (str) => {
            // 尝试匹配中文数字（第一章、第二章等）
            const chineseMatch = str.match(/第([一二三四五六七八九十]+)章/);
            if (chineseMatch) {
              const chineseNums = ['一', '二', '三', '四', '五', '六', '七', '八', '九', '十'];
              return chineseNums.indexOf(chineseMatch[1]) + 1;
            }
            
            // 尝试匹配阿拉伯数字（第1章、第2章等）
            const numMatch = str.match(/第(\d+)章/);
            if (numMatch) return parseInt(numMatch[1]);
            
            // 如果都不匹配，尝试提取纯数字
            const pureNum = str.match(/\d+/);
            return pureNum ? parseInt(pureNum[0]) : 0;
          };
          
          return getChapterNum(a._id) - getChapterNum(b._id);
        });
        
        chapters.value = processedData;
      } catch (error) {
        console.error('获取课程目录失败:', error);
      }
    };

    // 添加排序后的章节计算属性
    const sortedChapters = computed(() => {
      return [...filteredChapters.value].sort((a, b) => {
        // 提取中文数字
        const getChapterNum = (str) => {
          const match = str.match(/第([一二三四五六七八九十]+)章/);
          if (match) {
            const chineseNums = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九', '十'];
            return chineseNums.indexOf(match[1]);
          }
          // 如果没有匹配到中文数字，尝试提取阿拉伯数字
          const numMatch = str.match(/\d+/);
          return numMatch ? parseInt(numMatch[0]) : 0;
        };
        
        return getChapterNum(a._id) - getChapterNum(b._id);
      });
    });

    // 修改打开PDF方法，在本页面显示
    const openPdf = (pdfUrl) => {
      selectedPdf.value = pdfUrl; // 设置当前选中的PDF
    };

    // 添加关闭PDF方法
    const closePdf = () => {
      selectedPdf.value = null;
    };

    onMounted(() => {
      loadClassList();
    });

    // 过滤出当前课程的章节
    const filteredChapters = computed(() => {
      return chapters.value.filter(chapter => 
        chapter.info[0].subject === currentClass.value
      );
    });

    

    const sliderStyle = computed(() => {
      const tabWidth = 100 / 2;
      return {
        width: `${tabWidth}%`,
        transform: `translateX(${model.value * 100}%)`
      };
    });

    return {
      currentClass,
      model,
      filteredChapters,
      openPdf,
      sliderStyle,
      selectedPdf,
      closePdf,
      sortedChapters
    };
  }
};
</script>

<style>
/* 修改返回按钮样式 */
.back-button {
  position: absolute;
  right: 20px;  /* 距离右侧20px */
  top: 20px;    /* 距离顶部20px */
  z-index: 3;
}

/* 调整标签容器位置 */
.tab-container {
  position: relative;
  padding-right: 60px; /* 为右侧按钮留出空间 */
  display: flex;
  background: #f5f5f5;
  border-radius: 8px;
  padding: 8px;
  margin-bottom: 10px; /* 减小下边距 */
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

/* 添加卡片样式 */
.v-card {
  border-radius: 8px;
  box-shadow: 0 3px 5px rgba(0,0,0,0.1);
}

/* 添加列表容器样式 */
.list-container {
  background: white;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.chapter-header {
  background-color: #f5f5f5;
  border-radius: 4px;
  margin-bottom: 4px;
  transition: all 0.3s ease;
}

.chapter-header:hover {
  background-color: #e8f5e9;
}

.chapter-title {
  font-weight: 500;
  color: #2e7d32;
}

.topic-item {
  border-left: 3px solid #4caf50;
  margin-left: 12px;
  margin-bottom: 4px;
  transition: all 0.2s ease;
}

.topic-item:hover {
  background-color: #f1f8e9;
  transform: translateX(4px);
}

.topic-title {
  color: #424242;
}

/* 保持原有标签页样式不变... */
</style>
