<script setup lang="ts">
import { type Lyric } from 'lrc-kit'

const props = defineProps<{
  lyric: Lyric
}>()
</script>

<script lang="ts">
export default {
  mounted() {
  },
  methods: {

  },
}
</script>

<template>
  <p>{{ lyric.content }}</p>
</template>
