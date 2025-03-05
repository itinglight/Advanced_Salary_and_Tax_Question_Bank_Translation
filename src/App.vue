<script setup>
import { ref ,onMounted} from "vue";
import JsonData from "./assets/10.json"
import JsonData5_1 from "./assets/5_1.json"
import Question from "./components/Question.vue"
import questionData from "./assets/question.json"
let questionDataProcess = questionData.result.categoryList.reverse();
let questionList = JsonData.result.moduleList.questionList
let JsonDataStr = ref(JSON.stringify(JsonData))

onMounted(async () => {
  console.log("1111")
  JsonDataStr.value = JSON.stringify(JsonData)
  // console.log(JsonDataStr.value)
})
const change = (id)=>{
  console.log("change",id)
  JsonDataStr.value = JSON.stringify(JsonData5_1)
}
</script>

<template>
  <div v-once  class="header hidden-element box"  v-show="true">
    <div v-for="item in questionDataProcess"> 
      <h2 >{{ item.categoryName + item.totalQuestionCount}} </h2>
      <div v-if="item.totalQuestionCount>=100" class="hover" v-for="its in item.children.reverse()" @click="change(its.id)" >
        {{ its.categoryName }}
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
.box{
  border:2px solid red;
}
.hover:hover{
  background-color: yellow;
}
</style>
