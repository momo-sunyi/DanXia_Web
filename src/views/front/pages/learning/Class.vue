<template>
  <v-app class="green lighten-5">
    <v-container fluid>
      <v-row>
        <v-card
          class="pull-left ml-5 mt-5"
          height="500px"
          min-width="11%"
          id="l"
        >
          <v-list>
            <v-list-item-group v-model="model">
              <router-link
                :to="{
                  name: 'ClassIntro',
                  params: { currentClass: currentClass },
                }"
                class="text-decoration-none"
              >
                <v-list-item link>
                  <v-list-item-icon>
                    <v-icon>mdi-pen</v-icon>
                  </v-list-item-icon>
                  <v-list-item-content>
                    <v-list-item-title>课程介绍</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </router-link>
              <router-link
                :to="{
                  name:'ClassList',
                  params: { currentClass: currentClass },
                }"
                class="text-decoration-none"
              >
                <v-list-item link>
                  <v-list-item-icon>
                    <v-icon>mdi-book</v-icon>
                  </v-list-item-icon>
                  <v-container>
                  <vue-pdf-embed :source="pptsrc" />
                  </v-container>
                  <v-list-item-content>
                    <v-list-item-title>课程内容</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </router-link>
              <router-link
                to="/front/pages/learning/DisscusList"
                class="text-decoration-none"
              >
                <!-- <v-list-item link>
                  <v-list-item-icon>
                    <v-icon>mdi-message</v-icon>
                  </v-list-item-icon>
                  <v-list-item-content>
                    <v-list-item-title>讨论区</v-list-item-title>
                  </v-list-item-content>
                </v-list-item> -->
              </router-link>
            </v-list-item-group>
          </v-list>
        </v-card>

        <div id="r">
          <div class="mx-auto fill-height fill-width">
            <v-card class="mx-auto pa-8" min-height="100%" min-width="100%">
              <v-row align="start" no-gutters>
                <router-link
                  :to="{
                    name: 'ClassList',
                    params: { currentClass: currentClass },
                  }"
                  class="text-decoration-none"
                >
                  <v-subheader
                    class="font-weight-black black--text text-subtitle-1"
                    >>所有内容</v-subheader
                  >
                </router-link>
                <v-subheader>专题 </v-subheader>
                <v-col cols="2" class="mt-1">
                  <v-select
                    @change="SelectChange($event)"
                    v-model="ztname"
                    :items="ZTname"
                    menu-props="auto"
                    label="select"
                    outlined
                    dense
                    single-line
                  ></v-select>
                </v-col>

                <v-subheader>内容 </v-subheader>

                <v-col cols="2" class="mt-1">
                  <v-select
                    @change="change($event)"
                    v-model="nrname"
                    :items="NRname"
                    menu-props="auto"
                    outlined
                    dense
                    label="select"
                    single-line
                  ></v-select>
                </v-col>
              </v-row>
              <!-- video版块 -->
              <div fill-height outlined class="mx-3">
                <v-tabs
                  background-color="grey lighten-3 "
                  fixed-tabs
                  height="35"
                >
                  <v-tab href="#ppt" @click="flag = true">
                    <v-icon>mdi-laptop</v-icon>
                  </v-tab>

                  <!-- <v-tab href="#video" @click="flag = false">
                      <v-icon>mdi-video</v-icon>
                    </v-tab> -->
                </v-tabs>
               
                <!-- 展示内容 -->
                <div>
                  <v-card height="750">
                    <iframe :src="pSrc" width="100%" height="100%"></iframe>
                  </v-card>
                </div>
              </div>
            </v-card>
          </div>
        </div>
      </v-row>
    </v-container>
  </v-app>
</template>


<script>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';

export default {
  name: "Class",
  setup() {
    const route = useRoute();
    const router = useRouter();
    const currentClass = ref("自然地理");
    const pSrc = ref("");
    
    // 预设课程资料
    const presetMaterials = {
      "自然地理": {
        "第一章": [
          {
            text: "自然地理基础",
            ppt: "https://example.com/nature-geo.pdf"
          }
        ]
      },
      "人文地理": {
        "第一章": [
          {
            text: "人文地理概述", 
            ppt: "https://example.com/human-geo.pdf"
          }
        ]
      }
    };

    // 加载PPT数据
    const loadPPTData = () => {
      const course = route.params.currentClass || "自然地理";
      const chapter = route.params.parentName || "第一章";
      
      if (presetMaterials[course] && presetMaterials[course][chapter]) {
        const materials = presetMaterials[course][chapter];
        const selected = materials.find(item => item.text === route.params.currentName);
        
        if (selected) {
          pSrc.value = selected.ppt;
        }
      }
    };

    onMounted(() => {
      currentClass.value = route.params.currentClass || "自然地理";
      loadPPTData();
    });

    return {
      currentClass,
      pSrc
    };
  }
};
</script>
<style>
#r {
  min-height: 900px;
  min-width: 80%;
  position: relative;
  box-shadow: 0px 0px 3px rgb(160, 157, 157);
  left: 20px;
  top: 20px;
  margin-bottom: 40px;
  border-radius: 3%;
}
</style>
