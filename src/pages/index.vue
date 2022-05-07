<script setup lang="ts">

interface BlockState {
  x: number
  y: number
  mines?: boolean
  opened?: boolean
  count: number
}
const width = 10
const height = 10
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

    directionFn(block).forEach((b) => {
      if (b.mines)
        block.count++
    })
  }))
}

function directionFn(block: BlockState) {
  return direction.map(([dx, dy]) => {
    const x2 = block.x + dx
    const y2 = block.y + dy
    if (x2 < 0 || x2 >= width || y2 < 0 || y2 >= height)
      return undefined

    return XYList[x2][y2]
  }).filter(Boolean) as BlockState[]
}

function styleFn(block: BlockState) {
  if (!block.opened)
    return ''

  const minesStyle = block.mines && 'bg-red-500/10'

  return `${textType[block.count]} ${minesStyle}`
}

function click(block: BlockState) {
  if (block.opened)
    return

  if (block.mines) {
    alert('this is mines!')
    return
  }
  expendZero(block)
  if (!block.opened)
    block.opened = true
}

function expendZero(block: BlockState) {
  if (block.count)
    return

  directionFn(block).forEach((s) => {
    if (!s.opened) {
      s.opened = true
      expendZero(s)
    }
  })
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
