<template>
      <div v-if="JsonData.result" >
      <h1 > {{ JsonData.result.title }} 
        <div class="hidden-element btn">
            <button class="btn " @click="showAnswerFn">{{ !showAnswer?'查看答案':'隐藏答案' }}</button> 
            <button v-if="showAnswer" class="btn" style="margin: 0px 10px;" @click="showAnalysisFn">{{ !showAnalysis?'查看解析':'隐藏解析' }}</button>
        </div>
    </h1> 
      <!-- 回答 -->
       <div v-for="(list,index1) in JsonData.result.moduleList">
           <h2 v-if="index1==1">多选题</h2>
           <h2 v-if="index1==2">简答题</h2>
           <div v-for="(ls ,index) in list.questionList">
            
           <h4 style="margin:5px 0px 3px 0px;">{{ index+1 }}. <span v-html="ls.title"></span><span v-if="showAnswer"  class="answer"><span v-if="!ls.subQuestionList">答案: </span> {{ ls.questionAnswer.answer }}</span></h4>  
           <p   v-html="ls.option"></p>
           <!-- JSONData id >12.1 -->
            <div v-if="ls.subQuestionList" >
                <div v-for="(lss,index ) in ls.subQuestionList">
                    <div style="display: flex; font-weight: 800;">
                      {{ index+1 }}. <span v-html="lss.title" ></span>
                    </div>
                    <div style="display: flex;">
                      答案: <span v-html="lss.questionAnswer.answer"></span>
                    </div>
                    <p v-if="showAnswer&&showAnalysis" v-html="lss.questionAnswer.analysis" class="analysis"></p>
                </div>
              
            </div>
            <!--  -->
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