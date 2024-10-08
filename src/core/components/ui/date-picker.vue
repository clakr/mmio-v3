<script setup lang="ts">
import { Button } from '@/core/components/ui/button'
import { Calendar } from '@/core/components/ui/calendar'
import { Popover, PopoverContent, PopoverTrigger } from '@/core/components/ui/popover'
import cn from '@/core/utils/cn'
import { CalendarDate, DateFormatter, getLocalTimeZone } from '@internationalized/date'
import { ref } from 'vue'

const props = defineProps<{ initialValue?: Date }>()
const emit = defineEmits(['update:modelValue'])

const date = props.initialValue ?? new Date()
const year = date.getFullYear()
const month = date.getMonth() + 1 // index-based
const day = date.getDate()

const value = ref(new CalendarDate(year, month, day))

const df = new DateFormatter('en-US', { dateStyle: 'long' })
</script>

<template>
  <Popover>
    <PopoverTrigger as-child>
      <Button
        variant="outline"
        :class="
          cn('flex justify-start gap-x-2 text-left font-normal', !value && 'text-muted-foreground')
        "
      >
        <i class="bx bx-calendar text-lg" />
        {{ value ? df.format(value.toDate(getLocalTimeZone())) : 'Pick a date' }}
      </Button>
    </PopoverTrigger>
    <PopoverContent class="w-auto p-0">
      <Calendar
        v-model="value"
        initial-focus
        @update:model-value="emit('update:modelValue', $event?.toDate(getLocalTimeZone()))"
      />
    </PopoverContent>
  </Popover>
</template>
