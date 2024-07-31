<template>
  <a :href="src" target="_blank" rel="noopener noreferrer" class="underline-offset-4 underline font-semibold">{{ label
    }}</a>
</template>


<script setup lang="ts">
defineProps<{
  src: string
  label: string
}>()
</script>
