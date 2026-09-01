<template>
  <q-input class="q-pa-md" dense :label="label" v-model="model" inputmode="numeric"></q-input>
</template>

<script setup lang="ts">
import type { PromptpayInputProps } from './PromptpayInput.types'

withDefaults(defineProps<PromptpayInputProps>(), {})

const model = defineModel({ type: Number })
</script>
