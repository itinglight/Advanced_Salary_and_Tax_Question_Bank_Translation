<template>
      <div v-if="JsonData.result" >
      <h1 class="title"> {{ JsonData.result.title }} 
        <div class="hidden-element btn">
            <button class="btn " @click="showAnswerFn">{{ !showAnswer?'查看答案':'隐藏答案' }}</button> 
            <button v-if="showAnswer" class="btn" style="margin: 0px 10px;" @click="showAnalysisFn">{{ !showAnalysis?'查看解析':'隐藏解析' }}</button>
        </div>
    </h1> 
      <!-- 回答 -->
       <div v-for="list in JsonData.result.moduleList">
           <div v-for="(ls ,index) in list.questionList">
           <h4 style="margin:5px 0px 3px 0px;">{{ index+1 }}. <span v-html="ls.title"></span><span v-if="showAnswer"  class="answer">答案: {{ ls.questionAnswer.answer }}</span></h4>  
           <p  v-if="!showAnswer" v-html="ls.option"></p>
           <p v-if="showAnswer&&showAnalysis" v-html="ls.questionAnswer.analysis" class="analysis"></p>
           </div>
       </div>
  </div>

</template>
<script setup >
import { ref ,onMounted,watchEffect} from "vue";
let props = defineProps(['data'])
let JsonData= ref([])
let showAnswer =ref(false) //查看答案
let showAnalysis =ref(false)
onMounted(() => {
  console.log("onMounted")
  JsonData.value = JSON.parse(props.data)
  console.log()
})
watchEffect(()=>{
    JsonData.value = JSON.parse(props.data)
    console.log("watching...")
})
const showAnswerFn = () => {
    console.log("hello")
    showAnswer.value= !showAnswer.value
}

const showAnalysisFn = ()=>{
    showAnalysis.value = !showAnalysis.value
}


// let jsondata = JSON.parse(props.data)
</script>
<style  scoped>
@media print{
  .hidden-element {
    display: none;
  }
}
.btn{
    display: inline-block;
}
.answer{
    color: red;
}
.analysis{
   background: yellow;
}
</style>