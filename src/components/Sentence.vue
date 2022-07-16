<script setup lang="ts">
const props = defineProps<{
  lyric: string | undefined
}>()
</script>

<template>
  <p>{{ lyric }}</p>
</template>
