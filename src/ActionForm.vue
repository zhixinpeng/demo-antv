<template>
  <a-form :model="form" :label-col-props="{ span: 8 }" :wrapper-col-props="{ span: 16 }">
    <a-row>
      <a-col :span="24">
        <a-form-item label="动作名称">
          <a-input :value="form.nodeName" @change="handleNodeNameChange" />
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="设备类型">
          <a-select :value="form.deviceType" @change="handleDeviceTypeChange">
            <a-select-option :value="1">防火墙</a-select-option>
            <a-select-option :value="2">ISP</a-select-option>
          </a-select>
        </a-form-item>
      </a-col>
      <a-col :span="24">
        <a-form-item label="执行设备">
          <a-select :value="form.device" @change="handleDeviceChange">
            <a-select-option :value="1">研发防火墙</a-select-option>
            <a-select-option :value="2">公司防火墙</a-select-option>
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

const handleNodeNameChange = (e: any) => {
  emit('change', { ...form, nodeName: e.target.value })
}

const handleDeviceTypeChange = (e: number) => {
  emit('change', { ...form, deviceType: e })
}

const handleDeviceChange = (e: number) => {
  emit('change', { ...form, device: e })
}
</script>

<style lang="less" scoped></style>
