<template>
  <a-form
    ref="formRef"
    :model="form"
    :label-col-props="{ span: 8 }"
    :wrapper-col-props="{ span: 16 }"
  >
    <a-row>
      <a-col :span="24">
        <a-form-item field="nodeName" label="动作名称">
          <a-input v-model="form.nodeName" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="设备类型">
          <a-select v-model="form.name">
            <a-option :value="1">防火墙</a-option>
            <a-option :value="2">ISP</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="执行设备">
          <a-select v-model="form.name">
            <a-option :value="1">研发防火墙</a-option>
            <a-option :value="2">公司防火墙</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="执行接口">
          <a-select v-model="form.name">
            <a-option :value="1">新增</a-option>
            <a-option :value="2">删除</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="执行结果">
          <a-select v-model="form.name">
            <a-option :value="1">失败后退出</a-option>
            <a-option :value="2">失败后继续</a-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
  </a-form>
</template>

<script setup lang="ts">
import { ref } from 'vue'

/* ts类型定义区域 */
interface IForm {
  id: string
  nodeName: string
  name: string
  status: number
}

/* 定义数据区域 */
const form = ref<IForm>({
  id: '',
  name: '',
  nodeName: '',
  status: 1,
})

/* 事件处理区域 */

/* 监听 */

/* 生命周期 */
</script>

<style lang="less" scoped></style>
