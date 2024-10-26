<script setup>

import { ref, onMounted } from 'vue'
import useAPI from '@/composables/useAPI';
import { useRoute } from 'vue-router';
import BaseTitle from '@/components/BaseTitle.vue';
import DifficultyChip from '@/components/DifficultyChip.vue';
import MainScore from '@/components/MainScore.vue';
import useScore from '@/composables/useScore';
import router from '@/router';

const api = useAPI()
const question = ref(null)
const route = useRoute()

const answers = ref([])
const { changeScore } = useScore() 
const notification = ref ('')

onMounted(async () => {
  question.value = await api.getQuestion(route.params.id)

  answers.value.push({
    id: answers.value.length, 
    correct: true, 
    answer: question.value.correct_answer,
    points: question.value.difficulty === 'easy' ? 10 : question.value.difficulty === 'medium' ? 20 : 30, 
  })
  question.value.incorrect_answers.map((wrong_answer)=> {
    answers.value.push({
      id: answers.value.length, 
      correct: false, 
      answer: wrong_answer, 
      points: -5, 
    })
  })

  answers.value = shuffle(answers.value)
})

const shuffle = (array) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array 
}

const handleAnswer = (points) => {
  changeScore(points)

  if (points > 0) {
    notification.value ='CORRECT'
  }

  else {
    notification.value = 'INCORRECT'
  } 

  setTimeout(() => {
      router.push('/homework8/')
  }, 1000)
}

</script>

<template>
  <div v-if="question" class="flex h-full w-full flex-col items-center gap-8 p-10 text-slate-700">
    <BaseTitle> <MainScore></MainScore> &nbsp;
    
      <span class="font-bold" :class="notification==='CORRECT' ? 'text-green-400' : 'text-red-400'">
        {{ notification }}
      </span>
    
    </BaseTitle>
      <div v-html="question.question" class="text-center text-2xl font-bold text-slate-700" ></div>
        <div class="grid w-full flex-grow grid-cols-2 gap-8 hover:cursor-pointer">
          <div v-for="answer in answers" v-html="answer.answer" :key="answer.id" @click="handleAnswer(answer.points)"
            class="text-slate-700 text-4xl bg-purple-200 flex items-center justify-center rounded-lg py-10 px-2 hover:text-purple-900 trasition-colors duration-300">
          </div>
        </div>
          <DifficultyChip :difficulty="question.difficulty"></DifficultyChip>
      </div>
  <div v-else class="text-slate-700" >
    Loading...

  </div>
</template>
