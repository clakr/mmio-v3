<script setup lang="ts">
import { type AvatarVariants, avatarVariant } from '.'
import cn from '@/core/utils/cn'
import { AvatarRoot } from 'radix-vue'
import type { HTMLAttributes } from 'vue'

const props = withDefaults(
  defineProps<{
    class?: HTMLAttributes['class']
    size?: AvatarVariants['size']
    shape?: AvatarVariants['shape']
  }>(),
  {
    size: 'sm',
    shape: 'circle',
  },
)
</script>

<template>
  <AvatarRoot :class="cn(avatarVariant({ size, shape }), props.class)">
    <slot />
  </AvatarRoot>
</template>
