<template>
<label :for="`id`" :class="classes">
      <input :disabled="disabled" :id="`id`" type="radio" name="answer" @change="onchange" :value="value"  v-model="model">
      {{ value }}
     </label>
</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
  id:String,
  disabled: Boolean,
  value: String,
  correctAnswer: String

})
const emits = defineEmits(['change'])
const onChange =(event)=>{
  emits('change',event)
}
const model = defineModel()
const classes = computed(()=>({
  right: props.disabled && props.value === props.correctAnswer,
  wrong: props.disabled && props.value !== props.correctAnswer
}))
</script>

<style>
.right {
  color: green;

}

.wrong{
  color:red;
}
</style>