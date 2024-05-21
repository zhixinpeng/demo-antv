<script setup lang="ts">
import { Graph } from '@antv/x6'
import { Stencil } from '@antv/x6-plugin-stencil'
import { onMounted } from 'vue'

onMounted(() => {
  // 画布
  const graph: Graph = new Graph({
    container: document.getElementById('graph-container')!,
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
    mousewheel: {
      enabled: true,
      zoomAtMousePosition: true,
      modifiers: 'ctrl',
      minScale: 0.5,
      maxScale: 3,
    },
  })

  // 左侧拖拽节点
  const stencil = new Stencil({
    title: '流程图',
    target: graph,
    groups: [
      {
        title: '基础流程图',
        name: 'group1',
      },
    ],
  })
  document.getElementById('stencil')!.appendChild(stencil.container)

  // #region 初始化图形
  const ports = {
    groups: {
      top: {
        position: 'top',
        attrs: {
          circle: {
            r: 4,
            magnet: true,
            stroke: '#5F95FF',
            strokeWidth: 1,
            fill: '#fff',
            style: {
              visibility: 'hidden',
            },
          },
        },
      },
      right: {
        position: 'right',
        attrs: {
          circle: {
            r: 4,
            magnet: true,
            stroke: '#5F95FF',
            strokeWidth: 1,
            fill: '#fff',
            style: {
              visibility: 'hidden',
            },
          },
        },
      },
      bottom: {
        position: 'bottom',
        attrs: {
          circle: {
            r: 4,
            magnet: true,
            stroke: '#5F95FF',
            strokeWidth: 1,
            fill: '#fff',
            style: {
              visibility: 'hidden',
            },
          },
        },
      },
      left: {
        position: 'left',
        attrs: {
          circle: {
            r: 4,
            magnet: true,
            stroke: '#5F95FF',
            strokeWidth: 1,
            fill: '#fff',
            style: {
              visibility: 'hidden',
            },
          },
        },
      },
    },
    items: [
      {
        group: 'top',
      },
      {
        group: 'right',
      },
      {
        group: 'bottom',
      },
      {
        group: 'left',
      },
    ],
  }

  Graph.registerNode(
    'custom-rect',
    {
      inherit: 'rect',
      width: 66,
      height: 36,
      attrs: {
        body: {
          strokeWidth: 1,
          stroke: '#5F95FF',
          fill: '#EFF4FF',
        },
        text: {
          fontSize: 12,
          fill: '#262626',
        },
      },
      ports: { ...ports },
    },
    true,
  )

  const r1 = graph.createNode({
    shape: 'custom-rect',
    label: '开始',
    attrs: {
      body: {
        rx: 20,
        ry: 26,
      },
    },
  })

  const r2 = graph.createNode({
    shape: 'custom-rect',
    label: '动作节点',
  })

  const r3 = graph.createNode({
    shape: 'custom-rect',
    label: '审批节点',
  })

  const r4 = graph.createNode({
    shape: 'custom-rect',
    label: '结束',
    attrs: {
      body: {
        rx: 20,
        ry: 26,
      },
    },
  })

  stencil.load([r1, r2, r3, r4], 'group1')

  // 控制连接桩显示/隐藏
  const showPorts = (ports: NodeListOf<SVGElement>, show: boolean) => {
    for (let i = 0, len = ports.length; i < len; i += 1) {
      ports[i].style.visibility = show ? 'visible' : 'hidden'
    }
  }
  graph.on('node:mouseenter', () => {
    const container = document.getElementById('graph-container')!
    const ports = container.querySelectorAll('.x6-port-body') as NodeListOf<SVGElement>
    showPorts(ports, true)
  })
  graph.on('node:mouseleave', () => {
    const container = document.getElementById('graph-container')!
    const ports = container.querySelectorAll('.x6-port-body') as NodeListOf<SVGElement>
    showPorts(ports, false)
  })
})
</script>

<template>
  <div id="container">
    <div id="stencil"></div>
    <div id="graph-container"></div>
  </div>
</template>

<style scoped>
#container {
  display: flex;
  width: 100%;
  height: 100%;
}

#stencil {
  width: 200px;
  height: 100%;
  position: relative;
  border-right: 1px solid #dfe3e8;
}

#graph-container {
  flex: 1;
  height: 100%;
}
</style>
