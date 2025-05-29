<template>
  <div class="bg-grid min-h-screen text-gray-800">
    <div class="flex gap-8 mx-auto max-w-7xl">
      <!-- Main Content -->
      <main class="flex flex-col flex-1 gap-8">
        <PlatePreview 
          :plate-type="plateType"
          :font-style="fontStyle"
          :plate-number="plateNumber"
          :text-color="textColor"
        />
      </main>
    </div>
  </div>
</template>

<script setup>
import PlatePreview from '@/components/PlatePreview.vue'
import { ref } from 'vue'

const plateType = ref('standard')
const fontStyle = ref('euro')
const plateNumber = ref('')
const textColor = ref('#000000')


useHead({
  title: 'Malaysian e-Plate Generator',
  link: [
    {
      rel: 'icon',
      type: 'image/webp',
      href: '/images/icon.webp'
    }
  ]
})
</script>