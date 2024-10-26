<script setup>

const props = defineProps({
    difficulty: {
        type: String, 
        required: true, 
    }
})
</script>

<template>

    <p class="px-4 py-2 rounded-full border absolute uppercase text-white bottom-5 left-8 text-2xl font-bold" :class="props.difficulty === 'easy' ? 'bg-green-300' : props.difficulty === 'medium' ? 'bg-yellow-300' : 'bg-red-300'">
        {{ props.difficulty }}
    </p>

</template>