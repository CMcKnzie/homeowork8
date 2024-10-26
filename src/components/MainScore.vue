<script setup>

import useScore  from '@/composables/useScore';

const { score } = useScore()

</script>

<template>
    <span class="">Your Score: {{ score }}</span>
</template>
