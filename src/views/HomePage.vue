<script setup>
import { onMounted, ref } from 'vue'
import BaseTitle from '@/components/BaseTitle.vue';
import useAPI from '@/composables/useAPI'
import { RouterLink } from 'vue-router';
import MainScore from '@/components/MainScore.vue'

const { categories, getCategories } = useAPI()

onMounted(async () => {
  await getCategories()
})

</script>

<template>
<BaseTitle>Trivia Application- <MainScore></MainScore></BaseTitle>

<div class="text-slate-700 grid flex-grow grid-cols-4 gap-12 m-20">
<RouterLink v-for="category in categories" :key ="category.id"
:to="`/question/category/${category.id}`" 
class="bg-pink-100 text-center flex h-32 items-center justify-center rounded-lg border-4 border-pink-300 py-4 font-bold uppercase hover:cursor-pointer hover:border-purple-400 hover:bg-purple-200 hover:text-purple-900 trasition-colors duration-300"
>
  {{ category.name }}
</RouterLink>

  </div>
</template>
