<script setup lang="ts">
import { type ToastProps, toastVariants } from '.'
import cn from '@/core/utils/cn'
import { ToastRoot, type ToastRootEmits, useForwardPropsEmits } from 'radix-vue'
import { computed } from 'vue'

const props = defineProps<ToastProps>()

const emits = defineEmits<ToastRootEmits>()

const delegatedProps = computed(() => {
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  const { class: _, ...delegated } = props

  return delegated
})

const forwarded = useForwardPropsEmits(delegatedProps, emits)
</script>

<template>
  <ToastRoot
    v-bind="forwarded"
    :class="cn(toastVariants({ variant }), props.class)"
    @update:open="onOpenChange"
  >
    <slot />
  </ToastRoot>
</template>
