<template>
  <div class="wrap">
    <div class="content">
      <div class="text flex flex-row text-shadow px-8" v-for="n of repeat" :key="n">
        <div class="text-gray-100">{{ artist }}</div>
        <span class="px-2 text-white">-</span>
        <div class="font-semibold text-white mr-2">{{ title }}</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['artist', 'title', 'artwork'], 
  data () {
    return {
      repeat: 40
    }
  }
}
</script>
<style lang="css" scoped>
  @keyframes ticker {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-100%);
    }
  }
  .wrap {
    overflow: hidden;
  }
  .content {
    width: 10000000px;
  }
  .text {
    animation-name: ticker;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-play-state: running;
    animation-duration: 10s;
    float: left;
  }
</style>