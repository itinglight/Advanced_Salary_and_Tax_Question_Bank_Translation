<script setup>
import { ref ,onMounted} from "vue";
import JsonData from "./assets/1.json"
import Question from "./components/Question.vue"
import questionData from "./assets/question.json"

let questionList = JsonData.result.moduleList.questionList
let JsonDataStr = ref(JSON.stringify(JsonData))

let activeIndex = ref(1)
// 使用 import.meta.glob 动态加载 assets 目录下的所有 JSON 文件
const jsonModules = import.meta.glob('./assets/*.json');

// 创建一个对象，用于存储每个 JSON 文件的数据
const jsonDataArry = ref({});
let questionDataProcess = questionData.result.categoryList.reverse();
// 反转questionData.result.categoryListchildren.reverse()

questionDataProcess = questionData.result.categoryList.reverse();
onMounted(async () => {
  console.log("1111")
  // JsonDataStr.value = JSON.stringify(JsonData)
  // console.log(JsonDataStr.value)

  try {
    // 遍历所有匹配的 JSON 文件并加载
    for (const path in jsonModules) {
      try {
        console.log(`Loading: ${path}`);
        const module = await jsonModules[path](); // 动态导入 JSON 文件
        const fileName = path.split('/').pop().replace('.json', ''); // 提取文件名（去掉扩展名）
        jsonDataArry.value[fileName] = module.default; // 将 JSON 数据存储到 jsonDataArry 中
      } catch (error) {
        console.error(`Failed to load JSON file: ${path}`, error);
      }
    }
    console.log('JSON loading completed:', jsonDataArry.value);
  } catch (error) {
    console.error('Error during JSON loading:', error);
  }
})
const change = (id)=>{
  console.log("change",id,activeIndex.value)
  console.log("",jsonDataArry.value[id])
  activeIndex.value = id
  JsonDataStr.value = JSON.stringify(jsonDataArry.value[id])
}
</script>

<template>
  <div  class="header hidden-element box"  v-show="true">
    <div v-for="item in questionDataProcess"> 
      <div class="title" :class="{ active: activeIndex == item.id }" @click="change(item.id)">{{ item.categoryName }} <span class="count"> 共{{ item.totalQuestionCount }}道</span></div>
      <div v-if="item.totalQuestionCount>=100" class="t2" :class="{ active: activeIndex == its.id }" v-for="its in item.children.reverse()" @click="change(its.id)" >
        {{ its.categoryName }} <span class="count"> 共{{ its.totalQuestionCount }}道</span>
      </div>
    </div>
     
  </div>

  <Question :data="JsonDataStr"></Question>
</template>

<style>
@media print{
  .hidden-element{
    display: none!important;
  }
}
.count{
  display: inline-block;
  font-size: 10px;
  color: #00000073;
}
.title{
  font-weight: 900;
}
.active{
  background-color: #f69555;
}


.box{
  border:2px solid red;
  padding: 10px;
}

</style>
