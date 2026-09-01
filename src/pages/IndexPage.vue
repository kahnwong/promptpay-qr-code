<template>
  <q-page class="column items-center justify-evenly q-px-sm q-py-md">
    <div class="q-mt-md q-mb-lg">
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
    <div class="q-mb-md" style="width: 250px">
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
import PromptpayInput from '../components/PromptpayInput.vue'
import type { PromptpayInputProps } from '../components/PromptpayInput.types'
import { useQuasar } from 'quasar'

// qr code: config
const level = ref<Level>('M')
const renderAs = ref<RenderAs>('svg')

const gradient = ref(true)
const gradientType = ref<GradientType>('linear')
const gradientStartColor = ref('#000000')
const gradientEndColor = ref('#38bdf8')

// qr code: dynamic size
// Keep space for the form and the page's Quasar padding at every viewport size.
function useViewport() {
  const viewport = ref({ width: window.innerWidth, height: window.innerHeight })
  const syncViewport = () => {
    viewport.value = { width: window.innerWidth, height: window.innerHeight }
  }

  window.addEventListener('resize', syncViewport)
  onUnmounted(() => window.removeEventListener('resize', syncViewport))

  return viewport
}

const viewport = useViewport()
const maxQrcodeSize = computed(() => {
  if (viewport.value.width < 600) return 240
  if (viewport.value.width < 1024) return 320
  return 400
})

const qrcodeSize = computed(() =>
  Math.max(
    200,
    Math.floor(
      Math.min(viewport.value.width - 32, viewport.value.height - 220, maxQrcodeSize.value),
    ),
  ),
)

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
