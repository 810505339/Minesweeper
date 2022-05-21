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
  <div>
    <div v-for="(XList, x) in XYList" :key="x">
      <div v-for="(block, y) in XList" :key="y">
        {{ block }}
      </div>
    </div>
  </div>
</template>
