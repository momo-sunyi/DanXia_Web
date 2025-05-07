<template>
  <div style="margin: 0; padding: 0;">
    <v-img
      class="fill-height"
      src="@/assets/Learning.jpg"
      width="100%"
      :aspect-ratio="16 / 9"
    >
      <div style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.5);"></div>
      <v-main fluid style="margin: 0; padding: 0;">
        <v-container v-if="cards.length > 0" fill-height fluid class="mx-auto" style="margin: 0; padding: 0; display: flex; align-items: center; justify-content: center; margin-top: 200px;">  <!-- 添加 margin-top -->
          <v-row justify="center" align="center">
            <v-col cols="12" md="2" v-for="(card, index) in cards" :key="index">
              <v-hover v-slot="{ hover }" open-delay="200">
                <v-card 
                  class="mx-auto" 
                  width="200" 
                  height="260"  
                  link 
                  outlined 
                  :color="card.color" 
                  id="b"
                  :class="{ 'on-hover': hover }"  
                  :elevation="hover ? 16 : 3"
                  @click="card.clickHandler"
                >
                  <v-col class="fill-height" align="center" justify="center">
                    <v-row class="fill-height" align="center" justify="center">
                      <v-icon :color="card.iconColor" size="70">{{ card.icon }}</v-icon>
                    </v-row>
                    <v-text id="text1">{{ card.title }}</v-text>
                  </v-col>
                </v-card>
              </v-hover>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-img>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';  // 添加ref
import { useRouter } from 'vue-router';
import axios from 'axios';

export default {
  name: "FirstView",
  setup() {
    const router = useRouter();
    const cards = ref([]);  // 使用ref定义cards
    const currentClass = ref("自然地理");
    const GetMaterial = () => {
      axios({
        method: "get",
        url: "https://danxiagis.top:8081/uploadData/get",
      }).then(response => {
        let res = response.data;
        if (localStorage.getItem('meterial') != null && localStorage.getItem('meterial') != JSON.stringify(res)) {
          localStorage.setItem('meterial', JSON.stringify(res));
          console.log('不相同', JSON.parse(localStorage.getItem('meterial')));
        }
      });
    };
    
    const ClassPage1 = () => {
      router.push({
        name: "ClassList",
        params: { currentClass: "自然地理" },
      });
    };
    
    const ClassPage2 = () => {
      router.push({
        name: "ClassList",
        params: {
          currentClass: "人文与经济地理",
        },
      });
    };
    
    const ClassPage3 = () => {
      router.push({
        name: "ClassList",
        params: {
          currentClass: "地理信息系统原理",
        },
      });
    };
    
    const ClassPage4 = () => {
      router.push({
        name: "ClassList",
        params: {
          currentClass: "地图学",
        },
      });
    };
    
    const ClassPage5 = () => {
      alert("暂未开放，敬请期待");
      /* router.push({
        name: "ClassList",
        params: {
          currentClass: "其他",
        },
      }); */
    };
    
    // 将cards数组赋值给ref
    cards.value = [
      {
        icon: 'mdi-terrain',
        title: '自然地理',
        color: 'rgba(255, 255, 255, 0)',
        iconColor: 'white',
        clickHandler: ClassPage1
      },
      {
        icon: 'mdi-library',
        title: '人文与经济地理',
        color: 'rgba(255, 255, 255, 0)',
        iconColor: 'white',
        clickHandler: ClassPage2
      },
      {
        icon: 'mdi-earth',
        title: '地图学',
        color: 'rgba(255, 255, 255, 0)',
        iconColor: 'white',
        clickHandler: ClassPage4
      },
      {
        icon: 'mdi-city',
        title: '地理信息系统原理',
        color: 'rgba(255, 255, 255, 0)',
        iconColor: 'white',
        clickHandler: ClassPage3
      },
      {
        icon: 'mdi-bookmark',
        title: '其他资料',
        color: 'rgba(255, 255, 255, 0)',
        iconColor: 'white',
        clickHandler: ClassPage5
      }
    ];
    onMounted(() => {
      if (sessionStorage.getItem('isteacher') == 'true' ? false : true)
        GetMaterial();
    });
    
    return {
      GetMaterial,
      ClassPage1,
      ClassPage2,
      ClassPage3,
      ClassPage4,
      ClassPage5,
      cards
    };
  }
};
</script>

<style>
#text {
  color: rgb(255, 255, 255);
  font-size: 25px;
  font-family: "隶书", serif, serif;
  text-align: center;
  display: block;
  margin-top: -60px;
}
#text1{
  color: rgb(255, 255, 255);
  font-size: 21.8px;
  font-family: "隶书", serif, serif;
  text-align: center;
  display: block;
  margin-top: -60px;
}

#b:not(.on-hover) {
  opacity: 0.8;
}
</style>