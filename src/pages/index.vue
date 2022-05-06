<script setup lang="ts">

interface BlockState {
  x: number
  y: number
  mines?: boolean
  opened?: boolean
  count: number
}
const width = 5
const height = 5
const direction = [
  [1, -1],
  [1, 0],
  [1, 1],
  [0, -1],
  [0, 1],
  [-1, -1],
  [-1, 0],
  [-1, 1],
]
const textType = [
  'text-gray-400',
  'text-red-400',
  'text-green400',
  'text-cyan-400',
  'text-sky-400',
  'text-violet-400',
  'text-rose-400',

]
const XYList = $ref<BlockState[][]>(Array.from(Array(width), (_, x) => Array.from(Array(height), (_, y) => ({ x, y, count: 0 }))))

function generateMines() {
  XYList.forEach(blockList => blockList.forEach(block => block.mines = Math.random() < 0.1))
}

function updateCount() {
  XYList.forEach((blockList, x) => blockList.forEach((block, y) => {
    if (block.mines)
      return

    direction.forEach(([dx, dy]) => {
      const x2 = x + dx
      const y2 = y + dy
      if (x2 < 0 || x2 >= width || y2 < 0 || y2 >= height)
        return

      if (XYList[x2][y2].mines)

        block.count++
    })
  }))
}
function styleFn(block: BlockState) {
  if (!block.opened)
    return ''

  const minesStyle = block.mines && 'bg-red-500/10'

  return `${textType[block.count]} ${minesStyle}`
}

function click(block: BlockState) {
  if (!block.opened)
    block.opened = true
}
generateMines()
updateCount()

</script>

<template>
  <header m5 font-bold>
    MinesWeeper
  </header>
  <div v-for="(XList, x) in XYList" :key="x" flex="~" justify-center>
    <button v-for="(block, y) in XList" :key="y" m="0.5" w10 h10 flex justify-center items-center border="~ gray-400/40"
      hover="bg-gray-400/10" :class="styleFn(block)" @click="click(block)">
      <div v-if="block.opened" block>
        <div v-if="block.mines" i-mdi-mine />
        <div v-else>
          {{ block.count }}
        </div>
      </div>
    </button>
  </div>
</template>
