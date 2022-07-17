<script setup lang="ts">
import axios from 'axios'
import { type Lyric, Lrc as lrcParser } from 'lrc-kit'
import anime from 'animejs'
import { apiBase, translateY } from '../composables/config'
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
      animeTimeLine: anime.timeline({
        easing: 'easeInOutExpo',
        duration: 500,
      }),
    }
  },
  beforeCreate() {
  },
  beforeMount() {
    this.fetchLyrics()
  },
  methods: {
    fetchLyrics() {
      return axios.get(`${apiBase}/lyric?id=${this.id}`)
        .then((response) => {
          const lrc = lrcParser.parse(response.data.lrc.lyric)
          this.lyrics = lrc.lyrics
          this.isFetched = true
        })
    },
    animeLyrics() {
      const lyrics = this.$refs.lyric as any[]
      lyrics.forEach((el: any) => {
        this.animeTimeLine.add({
          targets: el.$el,
          translateY,
        }, el.$el.classList.value)
      })
      anime(this.animeTimeLine)
    },
  },
}
</script>

<template>
  <button bg-green @click="animeLyrics">
    start
  </button>
  <div v-if="isFetched">
    <Sentence v-for="(lyric, index) in lyrics" :key="index" ref="lyric" :lyric="lyric" :class="String(Math.floor(lyric.timestamp * 1000))" />
  </div>
  <!-- TODO:KEY -->
</template>
