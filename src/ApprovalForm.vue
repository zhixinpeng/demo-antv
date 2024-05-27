<template>
  <a-form :model="form" :label-col-props="{ span: 8 }" :wrapper-col-props="{ span: 16 }">
    <a-row>
      <a-col :span="24">
        <a-form-item label="节点名称">
          <a-input :value="form.nodeName" @change="handleNodeNameChange" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="审批人">
          <a-select :value="form.name" @change="handleNameChange">
            <a-select-option :value="1">张三</a-select-option>
            <a-select-option :value="2">李四</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="审批人">
          <a-radio-group type="button" :value="form.status" @change="handleStatusChange">
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
/* ts类型定义区域 */
interface IForm {
  type: string
  nodeName: string
  name?: number
  status?: number
}

/* 定义数据区域 */
const form = withDefaults(defineProps<IForm>(), {
  type: 'approval',
  nodeName: '审批节点',
  deviceType: undefined,
  device: undefined,
})

const emit = defineEmits(['change'])

const handleNodeNameChange = (e: any) => {
  emit('change', { ...form, nodeName: e.target.value })
}

const handleNameChange = (e: number) => {
  emit('change', { ...form, name: e })
}

const handleStatusChange = (e: any) => {
  emit('change', { ...form, status: e.target.value })
}
</script>

<style lang="less" scoped></style>
