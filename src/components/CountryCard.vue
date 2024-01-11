
<script setup>
import { FwbCard } from 'flowbite-vue'
import { stringify } from 'postcss'

defineProps({
  img: {
    type: String,
    required: true
  },
  name: {
    type: String,
    required: true
  }
})
</script>



<template>
  <fwb-card
    img-alt="Desk"
    :img-src=img
    variant="image"
  >
    <div class="p-5">
      <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
        {{ name }}
      </h5>
      <p class="font-normal text-gray-700 dark:text-gray-400">
        Here are the biggest enterprise technology acquisitions of 2021 so far, in reverse chronological order.
      </p>
    </div>
  </fwb-card>
</template>

