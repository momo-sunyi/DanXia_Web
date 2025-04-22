<template>
  <v-img
    id="bg_img"
    class="fill-height"
    src="../assets/新登录背景.png"
    :aspect-ratio="16 / 9"
    height="100vh"
    width="100vw"
  >
    <!-- 添加黑色透明度40%的蒙版 -->
    <div style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.6);"></div>

    <div class="flex-container">
      <v-card 
        id="draw-border" 
        auto 
        flat 
        v-show="loginshow" 
        style="border-radius: 20px;"
        :style="{ 
          width: show ? '400px' : '700px', 
          height: show ? '500px' : '300px',
          marginTop: show ? '-80px' : '0'  
        }" 
      >
        <v-row class="ml-14">
          <!-- 第一个卡片的 Logo -->
          <v-col cols="6" v-show="!show" :style="{ 
            marginTop: '-40px',
            marginLeft: '50px'
          }">
            <v-icon size="180px" color="#C8E6C9">mdi-terrain</v-icon>
            <h1 style="color: #C8E6C9; display: flex; width: 150px; justify-content: center; font-size: 30px; margin-top: -10px">虚拟丹霞</h1>
          </v-col>
        </v-row>

        <v-row class="ml-14">
          <v-col cols="6" v-show="!show">
            <button @click="visiter()">
              <v-icon right  size="50px"> mdi-account-cowboy-hat-outline</v-icon>
              <span>游客模式</span>
            </button>
            <button @click="enterlogin()" style="margin-bottom: 60px;margin-top: 20px;">
              <v-icon right size="50px"> mdi-face-man-shimmer-outline</v-icon>
              <span>账号登录</span>
            </button>
          </v-col>
          
          <v-col v-show="show" style="width: 100%; max-width: 500px; margin-left: -270px;">
            <div style="display: flex; flex-direction: column; align-items: center; gap: 20px; margin-top: -50px; width: 70%;">
              <!-- 添加 Logo 和 "虚拟丹霞" 文字 -->
              <div style="display: flex; flex-direction: column; align-items: center; margin-bottom: 20px;">
                <v-icon size="120px" color="#C8E6C9">mdi-terrain</v-icon>
                <h1 style="color: #C8E6C9; font-size: 25px; margin-top: px;">虚拟丹霞</h1>
              </div>
              
              <v-text-field
                label="学号"
                solo
                flat
                outlined
                prepend-inner-icon="mdi-account-outline"
                hide-details
                v-model="name"
                style="width: 100%;"
                class="white-input"
                color="#C8E6C9"
              ></v-text-field>
              
              <v-text-field
                label="密码"
                solo
                flat
                outlined
                prepend-inner-icon="mdi-lock-open"
                hide-details
                type="password"
                v-model="pwd"
                @keyup.enter="keydown"
                style="width: 100%;"
                class="white-input"
                color="#C8E6C9"
              ></v-text-field>
            </div>
            
            <div style="display: flex; justify-content: flex-start; margin-top: -0px; margin-left: 0px;">
              <v-btn
                width="100px"
                class="mr-3"
                color="primary"
                @click="login"
                :loading="loading"
                :disabled="loading"
                style="border-radius: 20px;"
              >登录</v-btn>
              <v-btn
                width="100px"
                class="ml-3"
                color="primary"
                @click="back"
                style="border-radius: 20px;"
              >返回</v-btn>
            </div>
          </v-col>
 
        </v-row>
      </v-card>
    </div>
  </v-img>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { useStore } from 'vuex';
import axios from "axios";

export default {
  name: "Login",
  setup() {
    const router = useRouter();
    const store = useStore();

    // 响应式状态
    const loginshow = ref(true);
    const show = ref(false);
    const name = ref("");
    const pwd = ref("");
    const checkbox = ref(true);
    const loading = ref(false);
    const isteacher = ref("");
    const tname = ref("");
    const err_msg = ref('');
    
    // API服务器配置 - 可以添加备用服务器
    const API_BASE_URL = ref('https://danxiagis.top:8081');
    const API_FALLBACK_URL = ref('http://danxiagis.top:8081'); // 备用不使用HTTPS的URL

    // 配置axios默认设置
    axios.defaults.timeout = 15000; // 请求超时时间
    axios.defaults.withCredentials = true; // 允许跨域携带cookie
    
    // 尝试使用备用URL发送请求
    const tryWithFallbackUrl = (url, config) => {
      // 将原URL替换为备用URL
      const fallbackUrl = url.replace(API_BASE_URL.value, API_FALLBACK_URL.value);
      config.url = fallbackUrl;
      
      console.log("尝试使用备用URL:", fallbackUrl);
      return axios(config);
    };

    // 方法
    const GetMaterial = () => {
      axios({
        method: "get",
        url: `${API_BASE_URL.value}/uploadData/get`,
      }).then((response) => {
        let res = response.data;
        localStorage.setItem("meterial", JSON.stringify(res));
      }).catch(error => {
        console.error("获取材料失败:", error);
      });
    };

    const enterlogin = () => {
      show.value = true;
    };

    const back = () => {
      show.value = false;
      loginshow.value = true;
    };

    const visiter = () => {
      sessionStorage.setItem("isvisiter", true);
      sessionStorage.setItem("userName", "游客");
      sessionStorage.setItem("loginState", true);
      router.push("/front");
    };

    const keydown = (e) => {
      if (e.keyCode === 13) {
        login();
      }
    };

    const login = () => {
      if (!name.value || !pwd.value) {
        alert("请输入学号和密码");
        return;
      }
      
      loading.value = true;
      
      // ======= 本地账号验证逻辑 =======
      // 优先检查是否为本地管理员或测试账号，避免发送远程请求
      // 管理员账号: 3admin/admin123
      if (name.value === "3admin" && pwd.value === "admin123") {
        console.log("使用本地管理员账号登录");
        
        // 创建模拟管理员数据
        const adminData = {
          name: "管理员账号",
          teacher_id: "3admin",
          Password: "admin123",
          // 添加可能缺少的必要字段
          class_name: "管理员班级",
          group: {
            group_id: "admin_group",
            leader: "3admin",
            members: ["管理员账号"]
          },
          // 兼容学生和教师数据结构
          Stu_id: "3admin"
        };
        
        // 设置所有会话数据
        sessionStorage.setItem("isvisiter", "false");
        sessionStorage.setItem("isteacher", "true");
        sessionStorage.setItem("userName", adminData.name);
        sessionStorage.setItem("class_name", adminData.class_name);
        sessionStorage.setItem("group_id", adminData.group.group_id);
        sessionStorage.setItem("group_leader", adminData.group.leader);
        sessionStorage.setItem("group_member", JSON.stringify(adminData.group.members));
        sessionStorage.setItem("stu_id", adminData.teacher_id);
        sessionStorage.setItem("password", adminData.Password);
        sessionStorage.setItem("loginState", "true");
        sessionStorage.setItem("is_leader", "true");
        
        // 存储到vuex
        store.state.stuName = adminData.name;
        store.commit("loginIn", adminData);
        
        // 去除加载效果
        loading.value = false;
        
        // 跳转到前端页面
        router.push("/front");
        return;
      }
      
      // 学生账号: student/123456
      if (name.value === "student" && pwd.value === "123456") {
        console.log("使用本地学生账号登录");
        
        // 创建模拟学生数据
        const studentData = {
          name: "学生测试账号",
          Stu_id: "student",
          Password: "123456",
          class_name: "2023级测试班",
          group: {
            group_id: "test_group",
            leader: "student",
            members: ["学生测试账号", "组员1", "组员2"]
          }
        };
        
        // 设置所有会话数据
        sessionStorage.setItem("isvisiter", "false");
        sessionStorage.setItem("isteacher", "false");
        sessionStorage.setItem("userName", studentData.name);
        sessionStorage.setItem("class_name", studentData.class_name);
        sessionStorage.setItem("group_id", studentData.group.group_id);
        sessionStorage.setItem("group_leader", studentData.group.leader);
        sessionStorage.setItem("group_member", JSON.stringify(studentData.group.members));
        sessionStorage.setItem("stu_id", studentData.Stu_id);
        sessionStorage.setItem("password", studentData.Password);
        sessionStorage.setItem("loginState", "true");
        sessionStorage.setItem("is_leader", "true");
        
        // 存储到vuex
        store.state.stuName = studentData.name;
        store.commit("loginIn", studentData);
        
        // 去除加载效果
        loading.value = false;
        
        // 跳转到前端页面
        router.push("/front");
        return;
      }
      
      // ======= 远程账号验证逻辑 =======
      // 第一个数字判断是不是老师，3是老师
      let f = name.value.substring(0, 1);
      //  教师和学生端判断
      if (f != 3) {
        sessionStorage.setItem("isteacher", "false");
        
        // 创建axios请求配置
        const axiosConfig = {
          method: "post",
          url: `${API_BASE_URL.value}/login/Web/student`,
          params: {
            Stu_id: name.value,
            Password: pwd.value,
          },
          withCredentials: true,
          timeout: 15000, // 增加超时时间
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
          }
        };
        
        axios(axiosConfig)
          .then((response) => {
            console.log(response);
            let dataObj = response.data;
            err_msg.value = dataObj.message;
            
            // 返回的学号无错误，即开始处理(后端进行了判断处理)
            if (dataObj.name != null) {
              // 将用户的信息写入sessionStorage中，方便后续需要展示信息时取来用
              sessionStorage.setItem("isvisiter", "false");
              sessionStorage.setItem("isteacher", "false");
              sessionStorage.setItem("userName", dataObj.name);
              sessionStorage.setItem("class_name", dataObj.class_name);
              sessionStorage.setItem("group_id", dataObj.group.group_id);
              sessionStorage.setItem("group_leader", dataObj.group.leader);
              sessionStorage.setItem("group_member", dataObj.group.members);
              sessionStorage.setItem("stu_id", dataObj.Stu_id);
              sessionStorage.setItem("password", dataObj.Password);
              sessionStorage.setItem("loginState", "true");
              if (dataObj.Stu_id == dataObj.group.leader)
                sessionStorage.setItem("is_leader", "true");
              
              store.state.stuName = dataObj.name;
              // 将用户信息放入vuex
              store.commit("loginIn", dataObj);
              
              // 去除加载效果
              loading.value = false;
              // 跳转导航页面
              router.push("/front");
            } else {
              loading.value = false;
              alert(response.data.message || "登录失败，请检查用户名和密码");
            }
          })
          .catch((error) => {
            console.error('登录请求错误:', error);
            
            // 尝试使用备用URL
            tryWithFallbackUrl(axiosConfig.url, axiosConfig)
              .then(response => {
                console.log("备用URL响应:", response);
                let dataObj = response.data;
                
                if (dataObj.name != null) {
                  // 将用户的信息写入sessionStorage中，方便后续需要展示信息时取来用
                  sessionStorage.setItem("isvisiter", "false");
                  sessionStorage.setItem("isteacher", "false");
                  sessionStorage.setItem("userName", dataObj.name);
                  sessionStorage.setItem("class_name", dataObj.class_name);
                  sessionStorage.setItem("group_id", dataObj.group.group_id);
                  sessionStorage.setItem("group_leader", dataObj.group.leader);
                  sessionStorage.setItem("group_member", dataObj.group.members);
                  sessionStorage.setItem("stu_id", dataObj.Stu_id);
                  sessionStorage.setItem("password", dataObj.Password);
                  sessionStorage.setItem("loginState", "true");
                  if (dataObj.Stu_id == dataObj.group.leader)
                    sessionStorage.setItem("is_leader", "true");
                  
                  store.state.stuName = dataObj.name;
                  // 将用户信息放入vuex
                  store.commit("loginIn", dataObj);
                  
                  // 去除加载效果
                  loading.value = false;
                  // 跳转导航页面
                  router.push("/front");
                } else {
                  loading.value = false;
                  alert(response.data.message || "登录失败，请检查用户名和密码");
                }
              })
              .catch(fallbackError => {
                loading.value = false;
                
                // 分析错误类型并给出相应提示
                if (error.response) {
                  // 服务器返回了错误状态码
                  alert(`登录失败: ${error.response.data.message || '服务器返回错误 ' + error.response.status}`);
                } else if (error.request) {
                  // 请求已发送但没有收到响应
                  alert("无法连接到服务器，请检查网络连接或稍后再试。您也可以尝试使用游客模式访问。");
                } else {
                  // 请求设置时出现问题
                  alert("登录请求出错: " + error.message);
                }
              });
          });
          
        // 将用户名和密码存在LocalStorage中
        localStorage.setItem("name", name.value);
        localStorage.setItem("pwd", pwd.value);
      }
      // 教师端登录
      else if (f == 3) {
        sessionStorage.setItem("isteacher", "true");
        
        // 创建axios请求配置
        const axiosConfig = {
          method: "post",
          url: `${API_BASE_URL.value}/login/smallWeb/teacher`,
          params: {
            teacher_id: name.value,
            Password: pwd.value
          },
          withCredentials: true,
          timeout: 15000, // 增加超时时间
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
          }
        };
        
        axios(axiosConfig)
          .then(response => {
            console.log(response);
            let dataObj = response.data;
            if (dataObj.name != null) {
              // 将用户的信息写入sessionStorage中，方便后续需要展示信息时取来用
              sessionStorage.setItem("isvisiter", "false");
              sessionStorage.setItem("userName", dataObj.name);
              sessionStorage.setItem("stu_id", dataObj.teacher_id);
              sessionStorage.setItem("password", dataObj.Password);
              sessionStorage.setItem("loginState", "true");
              
              store.state.stuName = dataObj.name;
              store.commit("loginIn", dataObj);
              
              // 去除加载效果
              loading.value = false;
              // 跳转导航页面
              router.push("/front");
            } else {
              loading.value = false;
              alert(response.data.message || "登录失败，请检查用户名和密码");
            }
          })
          .catch(error => {
            console.error('教师登录请求错误:', error);
            
            // 尝试使用备用URL
            tryWithFallbackUrl(axiosConfig.url, axiosConfig)
              .then(response => {
                console.log("备用URL响应:", response);
                let dataObj = response.data;
                
                if (dataObj.name != null) {
                  // 将用户的信息写入sessionStorage中，方便后续需要展示信息时取来用
                  sessionStorage.setItem("isvisiter", "false");
                  sessionStorage.setItem("userName", dataObj.name);
                  sessionStorage.setItem("stu_id", dataObj.teacher_id);
                  sessionStorage.setItem("password", dataObj.Password);
                  sessionStorage.setItem("loginState", "true");
                  
                  store.state.stuName = dataObj.name;
                  store.commit("loginIn", dataObj);
                  
                  // 去除加载效果
                  loading.value = false;
                  // 跳转导航页面
                  router.push("/front");
                } else {
                  loading.value = false;
                  alert(response.data.message || "登录失败，请检查用户名和密码");
                }
              })
              .catch(fallbackError => {
                loading.value = false;
                
                // 分析错误类型并给出相应提示
                if (error.response) {
                  // 服务器返回了错误状态码
                  alert(`登录失败: ${error.response.data.message || '服务器返回错误 ' + error.response.status}`);
                } else if (error.request) {
                  // 请求已发送但没有收到响应
                  alert("无法连接到服务器，请检查网络连接或稍后再试。您也可以尝试使用游客模式访问。");
                } else {
                  // 请求设置时出现问题
                  alert("登录请求出错: " + error.message);
                }
              });
          });
        
        // 将用户名和密码存在LocalStorage中
        localStorage.setItem("name", name.value);
        localStorage.setItem("pwd", pwd.value);
      }
    };

    // 刷新API地址（从HTTP尝试HTTPS）
    const refreshApiUrl = () => {
      if (API_BASE_URL.value.startsWith('https')) {
        API_BASE_URL.value = 'http://danxiagis.top:8081';
      } else {
        API_BASE_URL.value = 'https://danxiagis.top:8081';
      }
    };

    onMounted(() => {
      // 检测服务器连接状况
      axios.get(`${API_BASE_URL.value}/login/Web/health`, { timeout: 5000 })
        .catch(error => {
          console.log("主服务器不可访问，切换到备用URL");
          refreshApiUrl();
        });
    });

    return {
      loginshow,
      show,
      name,
      pwd,
      checkbox,
      loading,
      isteacher,
      tname,
      err_msg,
      GetMaterial,
      enterlogin,
      back,
      login,
      visiter,
      keydown
    };
  }
};
</script>
<style scoped>
.v-text-field .v-input__control input {
  color: #C8E6C9 !important;
}
</style>

<style scoped>
/* 禁用 v-btn 的伪元素动画 */
.v-btn::before,
.v-btn::after {
  content: none !important;
}

#bg_img {
  position: absolute;
}

#draw-border {
  background-color: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(3px);
  max-width: 1000px;
  width: 700px;
  height: 300px;
  margin: auto;
  transition: all 0.5s ease;
  display: flex; /* 添加 flex 布局 */
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
}

.flex-container {
  position: relative;
  top: 25%;
  justify-content: center;
  align-items: center;
  width: 100%; /* 确保宽度占满父容器 */
}

/* 调整按钮父容器样式 */
.v-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

button {
  width: 230px;
  height: 70px;
  margin-top: 50px;
  margin-bottom: 20px;
  border: 0;
  background: none;
  text-transform: uppercase;
  color: #C8E6C9; 
  font-weight: bold;
  position: relative;
  outline: none;
  padding: 10px 20px;
  box-sizing: border-box;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

button .v-icon {
  margin-right: 35px;
  color: white; /* 图标颜色改为白色 */
}

button span {
  font-size: 23px; /* 增大文字大小 */
}

button:hover {
  transform: translateY(-5px); /* 鼠标悬浮时按钮上移 */
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3); /* 添加阴影效果 */
}

button::before,
button::after {
  box-sizing: inherit;
  position: absolute;
  content: "";
  border: 2px solid transparent;
  width: 0;
  height: 0;
}

button::after {
  bottom: 0;
  right: 0;
}

button::before {
  top: 0;
  left: 0;
}

button:hover::before,
button:hover::after {
  width: 100%;
  height: 100%;
}

button:hover::before {
  border-top-color: #C8E6C9; 
  border-right-color: #C8E6C9; 
  transition: width 0.3s ease-out, height 0.3s ease-out 0.3s;
}

button:hover::after {
  border-bottom-color: #C8E6C9; 
  border-left-color: #C8E6C9; 
  transition: border-color 0s ease-out 0.6s, width 0.3s ease-out 0.6s,
    height 0.3s ease-out 1s;
}

/* 添加动画效果 */
@keyframes flow {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

/* 按钮悬停效果 */
.v-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 5px 15px rgba(32, 207, 137, 0.4);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  color: var(--clr);
  text-shadow: 0 0 15px var(--clr),
               0 0 40px var(--clr);
}

/* 应用到登录和返回按钮 */
.v-btn.primary:hover {
  --clr: #C8E6C9; /* 设置按钮的发光颜色 */
  animation: flow 3s ease-in-out infinite; /* 添加动画效果 */
}

/* 新增动效 */
.v-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  background: radial-gradient(var(--clr), transparent, transparent);
  opacity: 0;
  transition: .5s;
}

.v-btn:hover::before {
  opacity: 1;
}

.v-btn::after {
  content: '';
  position: absolute;
  inset: 2px;
  background: #272822;
}
</style>
