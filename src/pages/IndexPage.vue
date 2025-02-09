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
        :size="qrcodeSize"
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
import { ref, computed, onUnmounted } from 'vue'
import QrcodeVue from 'qrcode.vue'
import type { Level, RenderAs, GradientType } from 'qrcode.vue'
import generatePayload from 'promptpay-qr'
import PromptpayInput, { type PromptpayInputProps } from 'components/PromptpayInput.vue'
import { useQuasar } from 'quasar'

// qr code: config
const level = ref<Level>('M')
const renderAs = ref<RenderAs>('svg')

const gradient = ref(true)
const gradientType = ref<GradientType>('linear')
const gradientStartColor = ref('#000000')
const gradientEndColor = ref('#38bdf8')

// qr code: dynamic size
// <https://stackoverflow.com/a/76662078>
function useInnerWidth() {
  const width = ref(window.innerWidth)
  const syncWidth = () => (width.value = window.innerWidth)

  window.addEventListener('resize', syncWidth)
  onUnmounted(() => window.removeEventListener('resize', syncWidth))

  return width
}

const qrcodeSize = computed(() => {
  const width = useInnerWidth()

  let size = 500

  if (width.value <= 500) {
    size = 300
  }

  return size
})

// input struct
const $q = useQuasar()

export interface Promptpay {
  id: PromptpayInputProps
  amount: PromptpayInputProps
}

const promptpay = ref<Promptpay>({
  id: {
    label: 'PromptPay ID (Personal Only)',
    value: Number(String($q.localStorage.getItem('promptpayID') || '')),
  },
  amount: {
    label: 'Amount (THB)',
    value: Number(String($q.localStorage.getItem('promptpayAmount') || '')),
  },
})

// generate promptpay qr code
const promptpayPayload = computed(() => {
  // save to local storage
  $q.localStorage.set('promptpayID', promptpay.value.id.value)
  $q.localStorage.set('promptpayAmount', promptpay.value.amount.value)

  // generate payload
  const amount = Number(promptpay.value.amount.value)
  return generatePayload(String(promptpay.value.id.value), { amount })
})
</script>
