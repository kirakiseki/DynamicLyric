<script setup lang="ts">
import axios from 'axios'
import { type Lyric, Lrc as lrcParser } from 'lrc-kit'
defineProps<{
  id: number | string
}>()
</script>

<script lang="ts">
export default {
  data() {
    return {
      lyrics: [] as Lyric[],
      isFetched: false,
      apiBase: 'http://163.ishirai.cc',
    }
  },
  beforeCreate() {
  },
  mounted() {
    this.fetchLyrics()
  },
  methods: {
    fetchLyrics() {
      return axios.get(`${this.apiBase}/lyric?id=${this.id}`)
        .then((response) => {
          const lrc = lrcParser.parse(response.data.lrc.lyric)
          this.lyrics = lrc.lyrics
          this.isFetched = true
        })
    },
  },
}
</script>

<template>
  <div v-if="isFetched">
    <Sentence v-for="(lyric, index) in lyrics" :key="index" :lyric="lyric.content" />
  </div>
  <!-- TODO:KEY -->
</template>
