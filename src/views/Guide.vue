<template>
  <div style="background-color: rgba(239, 247, 233, 0.891)">
    <video autoplay loop muted
      style="width: 100%; height: 100vh; object-fit: cover; position: absolute; top: 0; left: 0;">
      <source src="/video/封面.mp4" type="video/mp4" />
      您的浏览器不支持视频标签。
    </video>

    <!-- 悬浮框 -->
    <!-- 悬浮框 -->
    <div
      style="position: absolute; top: 550px; right: 250px; background-color: rgba(255, 255, 255, 0); padding: 0; border-radius: 8px; z-index: 2; display: flex; width: 900px; height: 80px; align-items: center; justify-content: center;">
      <v-btn color="transparent" class="text-capitalize" to="/front/pages/route/route"
        style="font-size: 18px; color: #000; height: 100%; width: 225px; border-radius: 0; box-shadow: none;">
        普通视图
      </v-btn>
      <v-btn color="transparent" class="text-capitalize" to="/front/pages/route/route3"
        style="font-size: 18px; color: #000; height: 100%; width: 225px; border-radius: 0; box-shadow: none;">
        实习点详细
      </v-btn>
      <v-btn color="transparent" class="text-capitalize" to="/front/pages/download/download1"
        style="font-size: 18px; color: #000; height: 100%; width: 225px; border-radius: 0; box-shadow: none;">
        课程资料
      </v-btn>
      <v-btn color="transparent" class="text-capitalize" to="/front/pages/report/groupReport"
        style="font-size: 18px; color: #000; height: 100%; width: 225px; border-radius: 0; box-shadow: none; position: relative;">
        小组报告
        <div
          style="position: absolute; bottom: 0; left: 0; width: 100%; height: 4px; background-color: green; opacity: 0; transition: opacity 0.3s;">
        </div>
      </v-btn>
    </div>
  </div>

  <!-- 主页面内容 -->
  <v-sheet tile class="py-12" style="background-color: rgba(239, 247, 233, 0.891); margin-top: 90vh;">
    <v-container style="background-color: rgba(249, 251, 247, 0.863)">
      <!-- style="background-color: rgba(233, 246, 224, 0.863);" -->


      <div class="text-center my-12">
        <h1 class="text-h3 font-weight-bold mb-7">中国丹霞山</h1>
        <div
          style="width: 200px; height: 2px; background: linear-gradient(to right, rgba(144, 238, 144, 1), rgba(0, 128, 0, 1), rgba(144, 238, 144, 1)); margin: 10px auto;">
        </div>
        <div class="mt-15"> <!-- 新增的间距 div -->

          <v-carousel cycle height="400" hide-delimiters>
            <v-carousel-item v-for="(item, i) in carouselItems" :key="i">
              <v-row no-gutters style="height: 100%;">
                <!-- 左边图片 -->
                <v-col cols="6" style="height: 100%;">
                  <v-img :src="item.src" height="100%" cover></v-img>
                </v-col>
                <!-- 右边色块加文字 -->
                <v-col cols="6" :style="{ backgroundColor: item.bgColor }"
                  style="height: 100%; display: flex; align-items: center; justify-content: center;">
                  <div style="color: white; padding: 20px; text-align: left; width: 100%;">
                    <h3 class="text-h4 font-weight-bold" style="margin-bottom: 20px;">{{ item.title }}</h3>
                    <p class="text-body-1" style="white-space: pre-line;">{{ item.description }}</p>
                  </div>
                </v-col>
              </v-row>
            </v-carousel-item>
          </v-carousel>

        </div>

        <!-- 新增丹霞地貌演化过程部分 -->
        <div class="text-center my-12" style="margin-top: 48px ;">
          <h2 class="text-h3 font-weight-bold">丹霞地貌演化过程</h2>
          <div
            style="width: 200px; height: 2px; background: linear-gradient(to right, rgba(144, 238, 144, 1), rgba(0, 128, 0, 1), rgba(144, 238, 144, 1)); margin: 30px auto 48px;">
          </div>
          <v-row class="mt-6" justify="center">
            <v-col cols="12" md="3">
              <v-img src="../assets/中国东南丹霞地貌演化.gif" class="elevation-4" aspect-ratio="0.7" cover></v-img>
            </v-col>
            <v-col cols="12" md="3">
              <v-img src="../assets/重力坍塌形成陡崖.gif" class="elevation-4" aspect-ratio="0.7" cover></v-img>
            </v-col>
            <v-col cols="12" md="3">
              <v-img src="../assets/深切曲流差异风化.gif" class="elevation-4" aspect-ratio="0.7" cover></v-img>
            </v-col>
            <v-col cols="12" md="3">
              <v-img src="../assets/线状瀑布侵蚀.gif" class="elevation-4" aspect-ratio="0.7" cover></v-img>
            </v-col>
          </v-row>
        </div>

        <div class="text-center my-12" style="margin-top: 48px;">
          <h2 class="text-h3 font-weight-bold">通过这个网站你可以获得什么？</h2>
          <div
            style="width: 200px; height: 2px; background: linear-gradient(to right, rgba(144, 238, 144, 1), rgba(0, 128, 0, 1), rgba(144, 238, 144, 1)); margin: 35px auto;">
          </div>
        </div>
      </div>

      <!-- 功能按钮 -->
      <div class="my-12">
        <v-row class="py-12" justify="space-around">
          <!-- 左边大按钮 -->
          <v-col cols="12" md="4">
            <v-btn color="transparent" class="text-capitalize" large to="/front/pages/route/route" block
              style="height: 543px; font-size: 24px; position: relative; overflow: hidden;">
              <v-img :src="require('@/assets/模块1.png')" cover
                style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></v-img>
              <div style="position: absolute; bottom: 20px; right: 20px; z-index: 2;">
                <v-icon size="60" class="mr-2">mdi-map-marker-path</v-icon>
                实习路线
              </div>
              <!-- 黑色半透明蒙版及文字描述 -->
              <div class="hover-overlay">
                <div class="hover-text">
                  <span style="font-size: 26px; color: #90EE90;">了解实习线路及相关实习点</span><br>
                  <div style="height: 24px;"></div> <!-- 添加间隔 -->
                  <span style="font-size: 16px;">该模块展示了实习过程中的路线，<br>包括其中的实习点以及相关数据。</span>
                </div>
              </div>
            </v-btn>
          </v-col>

          <!-- 右边2*2按钮 -->
          <v-col cols="12" md="8">
            <v-row>
              <v-col cols="6">
                <v-btn color="transparent" class="text-capitalize" large to="/front/pages/cesium3d/tour" block
                  style="height: 260px; font-size: 18px; position: relative; overflow: hidden;">
                  <v-img :src="require('@/assets/模块2.png')" cover
                    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></v-img>
                  <div style="position: absolute; bottom: 10px; right: 20px; z-index: 2;">
                    <v-icon size="45" class="mr-2">mdi-earth</v-icon>
                    丹霞3D地形游览
                  </div>
                  <!-- 黑色半透明蒙版及文字描述 -->
                  <div class="hover-overlay">
                    <div class="hover-text">
                      <span style="font-size: 22px; color: #90EE90; ">沉浸式体验丹霞地貌3D地形</span><br>
                      <div style="height: 14px;"></div> <!-- 添加间隔 -->
                      <span style="font-size: 12px;">使用先进的Cesium 3D技术，探索丹霞地区的精细地形。<br>创建自定义飞行路线，从各个角度观察地貌特征，<br>获得身临其境的地质体验。</span>
                    </div>
                  </div>
                </v-btn>

              </v-col>
              <v-col cols="6">
                <v-btn color="transparent" class="text-capitalize" large to="/front/pages/download/download1" block
                  style="height: 260px; font-size: 18px; position: relative; overflow: hidden;">
                  <v-img :src="require('@/assets/模块3.png')" cover
                    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></v-img>
                  <div style="position: absolute; bottom: 10px; right: 20px; z-index: 2;">
                    <v-icon size="45" class="mr-2">mdi-book-open-page-variant</v-icon>
                    资料学习
                  </div>
                  <!-- 黑色半透明蒙版及文字描述 -->
                  <div class="hover-overlay">
                    <div class="hover-text">
                      <span style="font-size: 22px; color: #90EE90; ">充分的资料用于课前学习</span><br>
                      <div style="height: 14px;"></div> <!-- 添加间隔 -->
                      <span style="font-size: 12px;">该模块可以观看老师上传的学习资料，用于课前预习，<br>在预习中会记录学习时长，作为成绩考评的一部分。</span>
                    </div>
                  </div>
                </v-btn>
              </v-col>

              <v-col cols="6">
                <v-btn color="transparent" class="text-capitalize" large to="/front/pages/projectStudies/SectionLine"
                  block style="height: 260px; font-size: 18px; position: relative; overflow: hidden;">
                  <v-img :src="require('@/assets/模块4.png')" cover
                    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></v-img>
                  <div style="position: absolute; bottom: 10px; right: 20px; z-index: 2;">
                    <v-icon size="45" class="mr-2">mdi-tools</v-icon>
                    分析工具
                  </div>
                  <!-- 黑色半透明蒙版及文字描述 -->
                  <div class="hover-overlay">
                    <div class="hover-text">
                      <span style="font-size: 22px ; color: #90EE90 ">独特的分析处理工具</span><br>
                      <div style="height: 14px;"></div> <!-- 添加间隔 -->
                      <span style="font-size: 12px;">该模块包括剖面线工具、路径分析和可见性分析，<br>能够对实习路线及点位进行进一步分析。</span>
                    </div>
                  </div>
                </v-btn>
              </v-col>

              <v-col cols="6">
                <v-btn color="transparent" class="text-capitalize" large to="/front/pages/report/groupReport" block
                  style="height: 260px; font-size: 18px; position: relative; overflow: hidden;">
                  <v-img :src="require('@/assets/模块5.png')" cover
                    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;"></v-img>
                  <div style="position: absolute; bottom: 10px; right: 20px; z-index: 2;">
                    <v-icon size="45" class="mr-2">mdi-file-chart</v-icon>
                    实习报告
                  </div>
                  <!-- 黑色半透明蒙版及文字描述 -->
                  <div class="hover-overlay">
                    <div class="hover-text">
                      <span style="font-size: 22px; color: #90EE90;">实习成果的体现</span><br>
                      <div style="height: 14px;"></div> <!-- 添加间隔 -->
                      <span style="font-size: 12px;">该模块包括实习报告和拍照的一些注意事项，<br>并在实习结束后可以进行编辑，上传提交。</span>
                    </div>
                  </div>
                </v-btn>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </div>

    </v-container>
  </v-sheet>

</template>

<script>
import { ref, reactive, onMounted } from 'vue';

export default {
  setup() {
    const carouselItems = reactive([
      {
        src: require('@/assets/丹霞山1.png'),
        title: '阳元石',
        description: '天下第一奇石，雄伟壮丽，鬼斧神工。\n阳元石是丹霞山最具标志性的景观之一，高28米，直径7米，被誉为“天下第一奇石”。其造型逼真，历经30万年风雨侵蚀屹立不倒，堪称自然奇观。\n阳元石不仅是自然杰作，更蕴含深深的文化意义，体现了古人对生命与自然的敬畏。',
        bgColor: 'rgba(34, 139, 34, 0.7)', // 深绿色
      },
      {
        src: require('@/assets/丹霞山2.png'),
        title: '阴元石',
        description: '生命之门，神秘幽静，自然奇观。\n阴元石位于翔龙湖景区，造型独特，与阳元石遥相呼应，被誉为“大地之母”。其形成与阳元石类似，均为自然风化与侵蚀的杰作。\n阴元石与阳元石共同构成丹霞山的“阴阳奇观”，体现了自然界的和谐与平衡',
        bgColor: 'rgba(50, 205, 50, 0.7)', // 浅绿色
      },
      {
        src: require('@/assets/丹霞山3.png'),
        title: '长老峰',
        description: '丹霞之巅，日出云海，壮丽非凡。\n长老峰是丹霞山的主峰，海拔409米，因山势险峻、岩石嶙峋而得名。峰顶可俯瞰丹霞群峰全景，是观赏日出、云海的绝佳地点。',
        bgColor: 'rgba(0, 100, 0, 0.7)', // 深绿色
      },
      {
        src: require('@/assets/丹霞山4.png'),
        title: '锦江画廊',
        description: '碧水丹山，如诗如画，山水交融。\n翔龙湖因湖岸轮廓酷似一条腾飞的巨龙而得名，湖水清澈，四周丹崖碧水，景色如画。',
        bgColor: 'rgba(255, 215, 0, 0.7)', // 金色
      },
      {
        src: require('@/assets/丹霞山5.png'),
        title: '翔龙湖',
        description: '龙形碧水，湖光山色，美不胜收。\n锦江是丹霞山的母亲河，蜿蜒贯穿景区，形成“碧水丹山”的绝美画卷。乘船游览锦江，可近距离观赏两岸奇峰异石。',
        bgColor: 'rgba(60, 179, 113, 0.7)', // 中等绿色
      },
    ]);

    const isuser = ref(false);
    const visiter = ref(false);
    const isteacher = ref(false);
    const isstudent = ref(false);
    const d = ref(0);
    const personPages = reactive([
      {
        pp_isteacher: true,
      },
    ]);
    
    const setTime = () => {
      let timerForReckon = setInterval(function () {
        let xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function () {
          if (xhr.readyState === 4 && xhr.status === 200) {
            // console.log(new Date());
            // console.log("111");
          }
        };
        xhr.open("post", "https://danxiagis.top:8081/login/recordingTime"); 
        xhr.setRequestHeader(
          "Content-Type",
          "application/x-www-form-urlencoded"
        );
        xhr.send(`stu_id=${sessionStorage.getItem("stu_id")}`);
      }, 60000);
      
      let hidden, visibilityChange;
      if (typeof document.hidden !== "undefined") {
        hidden = "hidden";
        visibilityChange = "visibilitychange";
      } else if (typeof document.mozHidden !== "undefined") {
        hidden = "mozHidden";
        visibilityChange = "mozvisibilitychange";
      } else if (typeof document.msHidden !== "undefined") {
        hidden = "msHidden";
        visibilityChange = "msvisibilitychange";
      } else if (typeof document.webkitHidden !== "undefined") {
        hidden = "webkitHidden";
        visibilityChange = "webkitvisibilitychange";
      }

      // 添加监听器，在title里显示状态变化
      document.addEventListener(
        visibilityChange,
        function () {
          if (document[hidden]) {
            clearInterval(timerForReckon);
          } else {
            let date = new Date();
            let date1 = new Date(2020, 9, 5);
            d.value = Math.round((date.valueOf() - date1.valueOf()) / 86400000);
            
            timerForReckon = setInterval(function () {
              let xhr = new XMLHttpRequest();
              xhr.onreadystatechange = function () {
                if (xhr.readyState === 4 && xhr.status === 200) {
                  // console.log("222");
                }
              };
              xhr.open(
                "post",
                "https://danxiagis.top:8081/login/recordingTime"
              );
              xhr.setRequestHeader(
                "Content-Type",
                "application/x-www-form-urlencoded"
              );
              xhr.send(`stu_id=${sessionStorage.getItem("stu_id")}`);
            }, 60000);
          }
        },
        false
      );
    };
    
    onMounted(() => {
      setTime();
      
      let pageS = sessionStorage.getItem('isteacher');
      let visiterValue = sessionStorage.getItem('isvisiter');
      
      if (visiterValue == "true") {
        isteacher.value = false;
        isstudent.value = false;
        personPages[0].pp_isteacher = false;
      } else {
        visiter.value = true;
        isuser.value = true;
        
        if (pageS == "false") {
          isteacher.value = false;
          isstudent.value = true;
        } else {
          isteacher.value = true;
          isstudent.value = false;
          personPages[0].pp_isteacher = false;
        }
      }
    });
    
    return {
      carouselItems, // 确保 items 被返回
      isuser,
      visiter,
      isteacher,
      isstudent,
      d,
      personPages,
      setTime
    };
  }
};
</script>

<style scoped>
/* 悬浮蒙版样式 */
.hover-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0); /* 初始透明 */
  z-index: 3;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s;
}

/* 悬浮文字样式 */
.hover-text {
  color: white;
  font-size: 20px;
  opacity: 0; /* 初始透明 */
  transition: opacity 0.3s;
}

/* 悬浮效果 */
.v-btn:hover .hover-overlay {
  background-color: rgba(0, 0, 0, 0.6); /* 半透明黑色 */
}

.v-btn:hover .hover-text {
  opacity: 1; /* 显示文字 */
}


.carousel {
  perspective: 1200px; /* 定义3D空间的观察距离 */
}

.carousel-item {
  width: 500px; /* 根据需求调整 */
  height: 300px; /* 根据需求调整 */
  position: absolute;
  transition: transform 1s; /* 控制旋转过渡效果 */
  transform-style: preserve-3d; /* 保持3D空间中的子元素 */
}

.carousel-item img {
  width: 100%;
  height: 100%;
  backface-visibility: hidden; /* 隐藏背面，使图片看起来更平滑 */
}

/* 脉动动画效果 */
@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(244, 67, 54, 0.7);
    transform: scale(1);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(244, 67, 54, 0);
    transform: scale(1.05);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(244, 67, 54, 0);
    transform: scale(1);
  }
}

.v-btn:hover {
  color: rgb(113, 188, 113) !important;
}

.v-btn:hover div {
  opacity: 1;
}


/* 添加按钮背景图片样式 */
.v-btn {
  background-size: cover; /* 背景图片覆盖整个按钮 */
  background-position: center; /* 背景图片居中 */
  color: white !important; /* 文字颜色改为白色 */
  position: relative; /* 相对定位 */
  overflow: hidden; /* 隐藏溢出内容 */
}

/* 添加半透明遮罩，确保文字清晰 */
.v-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.1); /* 半透明黑色遮罩 */
  z-index: 1;
}

/* 确保文字在遮罩上方 */
.v-btn .v-btn__content {
  position: relative;
  z-index: 200;
}
</style>
