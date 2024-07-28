<template>
  <div class="border rounded p-2 my-2">
    <div class="w-full flex justify-end">
      <UiButton variant="ghost" size="icon" @click="onFullScreen">
        <Icon name="bi:arrows-fullscreen" size="18" />
      </UiButton>
    </div>

    <div class="w-full my-2 border-b-2"></div>
    <iframe :id="id" :src="src" allowfullscreen="true" height="600px" width="100%"></iframe>
  </div>

</template>


<script setup lang="ts">
defineProps<{
  src: string
  width?: string
  height?: string
}>()

const id = Math.random().toString(36).slice(2)


function onFullScreen() {
  document.getElementById(id)?.requestFullscreen()
}

</script>

<style scoped></style>