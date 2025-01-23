<template>
  <q-page class="column items-center justify-evenly">
    <div class="q-mt-lg">
      <qrcode-vue
        :value="value"
        :level="level"
        :render-as="renderAs"
        :gradient="gradient"
        :gradient-type="gradientType"
        :gradient-start-color="gradientStartColor"
        :gradient-end-color="gradientEndColor"
        :size="350"
      />
    </div>
    <div style="width: 250px">
      <PromptpayInput
        v-bind="promptpayID"
        :label="promptpayID.label"
        :v-model="promptpayID.value"
      />
      <PromptpayInput
        v-bind="promptpayAmount"
        :label="promptpayAmount.label"
        :v-model="promptpayAmount.value"
      />
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import QrcodeVue from 'qrcode.vue'
import type { Level, RenderAs, GradientType } from 'qrcode.vue'

// qr code config
const value = ref('foobarbaz')
const level = ref<Level>('M')
const renderAs = ref<RenderAs>('svg')

const gradient = ref(true)
const gradientType = ref<GradientType>('linear')
const gradientStartColor = ref('#000000')
const gradientEndColor = ref('#38bdf8')

// number input
import PromptpayInput, { type PromptpayInputProps } from 'components/PromptpayInput.vue'

const promptpayID = ref<PromptpayInputProps>({
  label: 'PromptPay ID (Personal Only)',
  value: '0000000000000',
})
const promptpayAmount = ref<PromptpayInputProps>({
  label: 'Amount (THB)',
  value: null,
})
</script>
