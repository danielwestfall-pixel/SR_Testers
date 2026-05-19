<template>
  <div class="paginator-container">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { ref, provide } from 'vue'

export interface PaginatedPage {
  id: string
  title: string
  content: string
}

interface Props {
  pages: PaginatedPage[]
  defaultPageId?: string
}

const props = withDefaults(defineProps<Props>(), {
  defaultPageId: undefined
})

const currentPageIndex = ref(props.defaultPageId ? props.pages.findIndex(p => p.id === props.defaultPageId) : 0)

const goToPage = (index: number) => {
  if (index >= 0 && index < props.pages.length) {
    currentPageIndex.value = index
  }
}

const goNext = () => {
  if (currentPageIndex.value < props.pages.length - 1) {
    currentPageIndex.value++
  }
}

const goPrev = () => {
  if (currentPageIndex.value > 0) {
    currentPageIndex.value--
  }
}

const canGoNext = () => currentPageIndex.value < props.pages.length - 1
const canGoPrev = () => currentPageIndex.value > 0

// Provide state to child components
provide('paginatorState', {
  pages: props.pages,
  currentPageIndex,
  goToPage,
  goNext,
  goPrev,
  canGoNext,
  canGoPrev
})
</script>

<style scoped>
.paginator-container {
  display: flex;
  flex-direction: column;
  height: 100%;
}
</style>
