<script setup lang="ts">
import cn from '@/core/utils/cn'
import { ToastTitle, type ToastTitleProps } from 'radix-vue'
import { type HTMLAttributes, computed } from 'vue'

const props = defineProps<ToastTitleProps & { class?: HTMLAttributes['class'] }>()

const delegatedProps = computed(() => {
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  const { class: _, ...delegated } = props

  return delegated
})
</script>

<template>
  <ToastTitle v-bind="delegatedProps" :class="cn('text-sm font-semibold', props.class)">
    <slot />
  </ToastTitle>
</template>
