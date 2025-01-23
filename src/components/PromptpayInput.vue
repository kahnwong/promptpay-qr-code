<template>
  <q-input class="q-pa-md" dense :label="label" v-model="model"></q-input>
</template>

<script setup lang="ts">
export interface PromptpayInputProps {
  label: string
  value: string | number | null
}

withDefaults(defineProps<PromptpayInputProps>(), {})

const model = defineModel({ type: String })
</script>
