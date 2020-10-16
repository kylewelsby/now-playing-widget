<template>
  <div class="flex flex-row items-center text-shadow">
    <div class="mr-4">
      <img :src="artwork" class="w-16 rounded-lg shadow-lg" />
    </div>
    <div>
      <div class="font-semibold text-white">{{ title }}</div>
      <div class="text-gray-100">{{ artist }}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['artist', 'title', 'artwork']
}
</script>