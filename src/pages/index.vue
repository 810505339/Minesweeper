<script setup lang="ts">
interface BlockState {
  x: number
  y: number
}
const width = 10
const height = 10
const XYList = $ref<BlockState[][]>(Array.from(Array(width), (_, x) => Array.from(Array(height), (_, y) => ({ x, y }))))

</script>

<template>
  {{ XYList }}
</template>
