<template>
  <q-page class="row items-center justify-evenly">
    <qrcode-vue
      :value="value"
      :level="level"
      :render-as="renderAs"
      :gradient="gradient"
      :gradient-type="gradientType"
      :gradient-start-color="gradientStartColor"
      :gradient-end-color="gradientEndColor"
      :size="500"
    />
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import QrcodeVue from 'qrcode.vue'
import type { Level, RenderAs, GradientType } from 'qrcode.vue'

const value = ref('foobarbaz')
const level = ref<Level>('M')
const renderAs = ref<RenderAs>('svg')

const gradient = ref(true)
const gradientType = ref<GradientType>('linear')
const gradientStartColor = ref('#000000')
const gradientEndColor = ref('#38bdf8')
</script>
