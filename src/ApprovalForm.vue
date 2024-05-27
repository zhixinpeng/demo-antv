<template>
  <a-form :model="form" :label-col-props="{ span: 8 }" :wrapper-col-props="{ span: 16 }">
    <a-row>
      <a-col :span="24">
        <a-form-item label="节点名称">
          <a-input :default-value="form.nodeName" @change="handleNodeNameChange" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="审批人">
          <a-select :default-value="form.name" @select="handleNameChange">
            <a-option :value="1">张三</a-option>
            <a-option :value="2">李四</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="审批人">
          <a-radio-group type="button" :default-value="form.status" @change="handleStatusChange">
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

const handleNodeNameChange = (value: any) => {
  emit('change', { ...form, nodeName: value })
}

const handleNameChange = (value: any) => {
  emit('change', { ...form, name: value })
}

const handleStatusChange = (value: any) => {
  emit('change', { ...form, status: value })
}
</script>

<style lang="less" scoped></style>
