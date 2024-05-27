<script setup lang="ts">
import { Graph, Shape, Node } from '@antv/x6'
import { Stencil } from '@antv/x6-plugin-stencil'
import { Transform } from '@antv/x6-plugin-transform'
import { Selection } from '@antv/x6-plugin-selection'
import { Snapline } from '@antv/x6-plugin-snapline'
import { Keyboard } from '@antv/x6-plugin-keyboard'
import { Clipboard } from '@antv/x6-plugin-clipboard'
import { History } from '@antv/x6-plugin-history'
import { onMounted, ref } from 'vue'
import ActionForm from './ActionForm.vue'
import ApprovalForm from './ApprovalForm.vue'

const selectedNode = ref<Node<Node.Properties>>()
const selectedNodeData = ref<any>()

let graph: Graph

onMounted(() => {
  initGraph()
})

// 画布初始化
const initGraph = () => {
  // 画布
  graph = new Graph({
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
    connecting: {
      router: 'manhattan',
      connector: {
        name: 'rounded',
        args: {
          radius: 8,
        },
      },
      anchor: 'center',
      connectionPoint: 'anchor',
      allowBlank: false,
      snap: {
        radius: 20,
      },
      // 连接线样式
      createEdge() {
        return new Shape.Edge({
          attrs: {
            line: {
              stroke: '#A2B1C3',
              strokeWidth: 2,
              targetMarker: {
                name: 'block',
                width: 12,
                height: 8,
              },
            },
          },
          zIndex: 0,
        })
      },
      // 验证是否连接上节点
      validateConnection({ targetMagnet }) {
        return !!targetMagnet
      },
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

  // 注册节点连接点
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

  // 注册节点图形
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

  // 创建预置节点
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
    data: {
      type: 'action',
      nodeName: '动作',
      deviceType: 1,
      device: 1,
    },
  })

  const r3 = graph.createNode({
    shape: 'custom-rect',
    label: '审批节点',
    data: {
      type: 'approval',
      nodeName: '审批',
      name: 1,
      status: 1,
    },
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

  // 左侧面板加载预置节点
  stencil.load([r1, r2, r3, r4], 'group1')

  // 控制连接桩显示/隐藏
  const showPorts = (ports: NodeListOf<SVGElement>, show: boolean) => {
    for (let i = 0, len = ports.length; i < len; i += 1) {
      ports[i].style.visibility = show ? 'visible' : 'hidden'
    }
  }

  // 画布注册一些事件
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
  bindGraphEvent()

  // 画布添加各种插件能力
  graph
    .use(
      new Transform({
        resizing: false,
        rotating: false,
      }),
    )
    .use(
      new Selection({
        rubberband: true,
        showNodeSelectionBox: true,
      }),
    )
    .use(new Snapline())
    .use(new Keyboard())
    .use(new Clipboard())
    .use(new History())

  // 注册各种键盘快捷键
  graph.bindKey(['meta+c', 'ctrl+c'], () => {
    const cells = graph.getSelectedCells()
    if (cells.length) {
      graph.copy(cells)
    }
    return false
  })
  graph.bindKey(['meta+x', 'ctrl+x'], () => {
    const cells = graph.getSelectedCells()
    if (cells.length) {
      graph.cut(cells)
    }
    return false
  })
  graph.bindKey(['meta+v', 'ctrl+v'], () => {
    if (!graph.isClipboardEmpty()) {
      const cells = graph.paste({ offset: 32 })
      graph.cleanSelection()
      graph.select(cells)
    }
    return false
  })
  graph.bindKey(['meta+z', 'ctrl+z'], () => {
    if (graph.canUndo()) {
      graph.undo()
    }
    return false
  })
  graph.bindKey(['meta+shift+z', 'ctrl+shift+z'], () => {
    if (graph.canRedo()) {
      graph.redo()
    }
    return false
  })
  graph.bindKey(['meta+a', 'ctrl+a'], () => {
    const nodes = graph.getNodes()
    if (nodes) {
      graph.select(nodes)
    }
  })
  graph.bindKey('backspace', () => {
    const cells = graph.getSelectedCells()
    if (cells.length) {
      graph.removeCells(cells)
    }
  })
  graph.bindKey(['ctrl+1', 'meta+1'], () => {
    const zoom = graph.zoom()
    if (zoom < 1.5) {
      graph.zoom(0.1)
    }
  })
  graph.bindKey(['ctrl+2', 'meta+2'], () => {
    const zoom = graph.zoom()
    if (zoom > 0.5) {
      graph.zoom(-0.1)
    }
  })
}

// 绑定画布事件，用来实现业务功能
// https://x6.antv.antgroup.com/tutorial/basic/events#%E9%BC%A0%E6%A0%87%E4%BA%8B%E4%BB%B6
const bindGraphEvent = () => {
  graph.on('node:selected', (e) => {
    const { node } = e
    selectedNode.value = node
    selectedNodeData.value = node.data
  })
}

const handleFormChange = (form: any) => {
  selectedNode.value?.updateData(form)
  selectedNodeData.value = form
}

const exportJson = () => {
  console.log(graph.toJSON())
  localStorage.setItem('graph', JSON.stringify(graph.toJSON()))
  // 导出的JSON存到服务端，让服务端从这里面拿自己要的数据
  // 回显时，服务端再下发这份JSON给前端，前端用 graph.fromJSON(json) 重新显示
}

const importJson = () => {
  const json = localStorage.getItem('graph')
  if (json) {
    graph.fromJSON(JSON.parse(json))
  }
}
</script>

<template>
  <div id="container">
    <div id="stencil"></div>
    <div id="graph-container"></div>
    <div id="nodes">
      <div class="content">
        <template v-if="selectedNodeData">
          <template v-if="selectedNodeData.type === 'action'">
            <ActionForm
              :key="selectedNode?.id"
              v-bind="selectedNodeData"
              @change="handleFormChange"
            />
          </template>
          <template v-if="selectedNodeData.type === 'approval'">
            <ApprovalForm
              :key="selectedNode?.id"
              v-bind="selectedNodeData"
              @change="handleFormChange"
            />
          </template>
        </template>
      </div>
      <a-flex vertical gap="small">
        <a-button @click="importJson">导入数据</a-button>
        <a-button type="primary" @click="exportJson">导出数据</a-button>
      </a-flex>
    </div>
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

#nodes {
  width: 300px;
  height: 100%;
  position: relative;
  border-left: 1px solid #dfe3e8;
  background-color: #f2f7fa;
  color: black;
  text-align: center;
  display: flex;
  flex-direction: column;
  padding: 40px 20px;
}

.content {
  width: 100%;
  flex: 1;
}
</style>
