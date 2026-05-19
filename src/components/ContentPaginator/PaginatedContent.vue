<template>
  <div class="paginated-content">
    <div aria-live="polite" aria-atomic="true" class="sr-only">
      {{ srAnnouncement }}
    </div>
    <div class="page-container">
      <div class="page-content" :aria-label="contentAriaLabel">
        <slot :page="currentPage">
          <h2 ref="pageHeadingRef" tabindex="-1">{{ currentPage.title }}</h2>
          <p>{{ currentPage.content }}</p>
        </slot>
      </div>
    </div>
    <div class="page-counter">
      {{ currentPageIndex + 1 }} / {{ pages.length }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { inject, computed, ref, watch } from 'vue'

interface PaginatedPage {
  id: string
  title: string
  content: string
}

interface PaginatorState {
  pages: PaginatedPage[]
  currentPageIndex: { value: number }
}

const paginatorState = inject<PaginatorState>('paginatorState')!

const pages = computed(() => paginatorState.pages)
const currentPageIndex = computed(() => paginatorState.currentPageIndex.value)
const currentPage = computed(() => pages.value[currentPageIndex.value])

const pageHeadingRef = ref<HTMLHeadingElement | null>(null)

const srAnnouncement = computed(() => {
  return `Page ${currentPageIndex.value + 1} of ${pages.value.length}: ${currentPage.value.title}`
})

const contentAriaLabel = computed(() => {
  return `Page content: ${currentPage.value.title} (${currentPageIndex.value + 1} of ${pages.value.length})`
})

// Focus the page heading when page changes
watch(currentPageIndex, () => {
  // Use setTimeout to ensure DOM has updated
  setTimeout(() => {
    pageHeadingRef.value?.focus()
  }, 0)
})
</script>

<style scoped>
.paginated-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.page-container {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 0;
}

.page-content {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  animation: fadeIn 0.3s ease-in;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.page-content h2 {
  margin: 0 0 20px 0;
  color: #0066cc;
  font-size: 32px;
  text-align: center;
  outline: none;
}

.page-content h2:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 4px;
  border-radius: 4px;
}

.page-content p {
  margin: 0;
  color: #555;
  font-size: 16px;
  line-height: 1.6;
  text-align: center;
  max-width: 600px;
}

.page-counter {
  text-align: center;
  padding: 15px;
  color: #999;
  font-size: 14px;
  background-color: #f9f9f9;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
</style>
