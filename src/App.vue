<template>
  <div class="h-screen">
    <NowPlaying />
  </div>
</template>

<script>
import NowPlaying from './components/NowPlaying.vue'

export default {
  name: 'App',
  components: {
    NowPlaying,
  }
}
</script>
