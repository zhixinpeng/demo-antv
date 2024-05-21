<script setup lang="ts">
import { Graph } from '@antv/x6'
import { onMounted } from 'vue'

let graph: Graph

onMounted(() => {
  graph = new Graph({
    container: document.getElementById('container') || undefined,
    width: 800,
    height: 600,
    background: {
      color: '#F2F7FA',
    },
    grid: {
      visible: true,
      type: 'doubleMesh',
      args: [
        {
          color: '#eee', // 主网格线颜色
          thickness: 1, // 主网格线宽度
        },
        {
          color: '#ddd', // 次网格线颜色
          thickness: 1, // 次网格线宽度
          factor: 4, // 主次网格线间隔
        },
      ],
    },
  })
})
</script>

<template>
  <div id="container"></div>
</template>

<style scoped></style>
