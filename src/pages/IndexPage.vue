<template>
  <q-page class="column items-center justify-evenly">
    <div class="q-mt-lg">
      <qrcode-vue
        :value="promptpayPayload"
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
        v-for="input in promptpay"
        :key="input.label"
        v-bind="input"
        :label="input.label"
        v-model="input.value"
      />
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import QrcodeVue from 'qrcode.vue'
import type { Level, RenderAs, GradientType } from 'qrcode.vue'
import generatePayload from 'promptpay-qr'
import PromptpayInput, { type PromptpayInputProps } from 'components/PromptpayInput.vue'

// qr code config
const level = ref<Level>('M')
const renderAs = ref<RenderAs>('svg')

const gradient = ref(true)
const gradientType = ref<GradientType>('linear')
const gradientStartColor = ref('#000000')
const gradientEndColor = ref('#38bdf8')

// input struct
export interface Promptpay {
  id: PromptpayInputProps
  amount: PromptpayInputProps
}
const promptpay = ref<Promptpay>({
  id: {
    label: 'PromptPay ID (Personal Only)',
    value: '0000000000000',
  },
  amount: {
    label: 'Amount (THB)',
    value: '',
  },
})

// generate promptpay qr code
const promptpayPayload = computed(() => {
  const amount = Number(promptpay.value.amount.value)
  return generatePayload(promptpay.value.id.value, { amount })
})
</script>
