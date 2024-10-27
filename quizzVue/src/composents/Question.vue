<template>
<div class="question">
<h3>{{ question.question }}</h3>
<ul>
  <li v-for="(choice, index) in randomChoices" :key="choice">
    <Answer :id="`answer${index}`" :disabled="hasAnswer" :value="choice" @change="onAnswer" v-model="answer":correctAnswer="question.correct_answer"/>
    
  </li>
</ul>

<button :disabled="!hasAnswer" @click="emits('answer',answer)">Question suivante</button>

</div>


</template><script setup>
import Answer from './Answer.vue';
import { shuffleArray } from '@/functions/array';
import {ref, computed, watch, onMounted, onUnmounted} from 'vue'
const props = defineProps({
  question : Object
})
const answer=ref(null)
const hasAnswer = computed(()=> answer.value !== null)
const emits = defineEmits(['answer']);
watch(()=> props.question, ()=>{
  answer.value = null
})
const randomChoices = computed (()=>shuffleArray(props.question.choice))
 const onAnswer = ()=>{
 
  clearTimeout(timer)
  timer = setTimeout(()=>{
    emits('answer', answer.value)
  },1_500)
 }


let timer

onMounted(()=>{
  timer =setTimeout(()=>{
   answer.value ='';
   onAnswer()
  },3_000) })

onUnmounted(()=>{
 clearTimeout(timer)

})

</script>

<style>
.question{
  padding-top:2rem;
}
.question button {
  margin-left: auto;
  display:block;
}

</style>