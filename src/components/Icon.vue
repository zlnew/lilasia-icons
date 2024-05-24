<script setup lang="ts">
import { StyleValue, computed } from 'vue'

type MaterialSymbolProps = {
  name: string
  filled?: boolean
  weight?: number | string
  grade?: number | string
  opticalSize?: number | string
  size?: number | string
}

const props = withDefaults(defineProps<MaterialSymbolProps>(), {
  filled: false,
  weight: 400,
  grade: 0,
  opticalSize: 48,
  size: 24
})

const style = computed<StyleValue>(() => {
  const $fill = Number(props.filled)
  const $wght = Number(props.weight)
  const $grad = Number(props.grade)
  const $opsz = Number(props.opticalSize)
  const $fontSize = Number(props.size)

  return {
    fontSize: `${$fontSize}px`,
    fontVariationSettings: `'FILL' ${$fill}, 'wght' ${$wght}, 'GRAD' ${$grad}, 'opsz' ${$opsz}`
  }
})
</script>

<template>
  <span :style="style" class="material-symbols-outlined">{{ name }}</span>
</template>
