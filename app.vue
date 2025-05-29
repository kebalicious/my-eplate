<template>
  <div class="bg-grid bg-white dark:bg-gray-900 min-h-screen text-gray-800 dark:text-gray-200">
    <div class="flex gap-8 mx-auto max-w-[96rem]">
      <!-- Main Content -->
      <main class="flex flex-col flex-1 gap-8">
        <PlatePreview :plate-type="plateType" :font-style="fontStyle" :plate-number="plateNumber"
          :text-color="textColor" />
      </main>
    </div>
  </div>
</template>

<script setup>
import PlatePreview from '@/components/PlatePreview.vue'
import { ref, onMounted } from 'vue'

const plateType = ref('standard')
const fontStyle = ref('euro')
const plateNumber = ref('')
const textColor = ref('#000000')

// Handle dark mode
onMounted(() => {
  // Check for saved theme preference
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark') {
    document.documentElement.classList.add('dark')
  } else if (savedTheme === 'light') {
    document.documentElement.classList.remove('dark')
  } else {
    // If no saved preference, check system preference
    if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
      document.documentElement.classList.add('dark')
    }
  }
})

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

<style>
/* Add dark mode styles for the grid background */
.bg-grid {
  background-color: #fafaf9;
  background-image:
    linear-gradient(to right, #e5e7eb 1px, transparent 1px),
    linear-gradient(to bottom, #e5e7eb 1px, transparent 1px);
  background-size: 32px 32px;
}

.dark .bg-grid {
  background-color: #111827;
  background-image:
    linear-gradient(to right, #1f2937 1px, transparent 1px),
    linear-gradient(to bottom, #1f2937 1px, transparent 1px);
  background-size: 32px 32px;
}
</style>