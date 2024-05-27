<template>
  <a-form :model="form" :label-col-props="{ span: 8 }" :wrapper-col-props="{ span: 16 }">
    <a-row>
      <a-col :span="24">
        <a-form-item label="动作名称">
          <a-input :default-value="form.nodeName" @change="handleNodeNameChange" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="设备类型">
          <a-select :default-value="form.deviceType" @select="handleDeviceTypeChange">
            <a-option :value="1">防火墙</a-option>
            <a-option :value="2">ISP</a-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="执行设备">
          <a-select :default-value="form.device" @select="handleDeviceChange">
            <a-option :value="1">研发防火墙</a-option>
            <a-option :value="2">公司防火墙</a-option>
          </a-select>
        </a-form-item>
      </a-col>
    </a-row>
  </a-form>
</template>

<script setup lang="ts">
interface IForm {
  type: string
  nodeName: string
  deviceType?: number
  device?: number
}

const form = withDefaults(defineProps<IForm>(), {
  type: 'action',
  nodeName: '动作节点',
  deviceType: undefined,
  device: undefined,
})

const emit = defineEmits(['change'])

const handleNodeNameChange = (value: any) => {
  emit('change', { ...form, nodeName: value })
}

const handleDeviceTypeChange = (value: any) => {
  emit('change', { ...form, deviceType: value })
}

const handleDeviceChange = (value: any) => {
  emit('change', { ...form, device: value })
}
</script>

<style lang="less" scoped></style>
