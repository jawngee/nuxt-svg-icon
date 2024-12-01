<script setup lang="ts">
// file imported from https://github.com/gitFoxCode/nuxt-icons/blob/89e53649e5868c31fc97869918ede96504ae1a04/src/runtime/components/nuxt-icon.vue
// with modifications
defineProps<{
  icon: object,
  name: string,
  width?: string,
  height?: string,
  fill?: string,
  stroke?: string,
}>();
</script>

<template>
  <component
    :is="icon"
    :style="{
      width: width || height ? (width ?? 'auto') : undefined,
      height: width || height ? (height ?? 'auto') : undefined,
      fill: fill && fill.length > 0 ? fill : undefined,
      stroke: stroke && stroke.length > 0 ? stroke : undefined,
    }"
  />
</template>
