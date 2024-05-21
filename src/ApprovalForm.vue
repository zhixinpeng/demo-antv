<template>
  <a-form
    ref="formRef"
    :model="form"
    :label-col-props="{ span: 8 }"
    :wrapper-col-props="{ span: 16 }"
  >
    <a-row>
      <a-col :span="24">
        <a-form-item field="nodeName" label="节点名称">
          <a-input v-model="form.nodeName" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="审批人">
          <a-select v-model="form.name">
            <a-option :value="1">张三</a-option>
            <a-option :value="2">李四</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item field="name" label="审批人">
          <a-radio-group type="button" v-model="form.status">
            <a-radio :value="1">邮件</a-radio>
            <a-radio :value="2">短信</a-radio>
            <a-radio :value="3">工单</a-radio>
          </a-radio-group>
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
