<template>
  <div class="paginated-content">
    <div class="page-container">
      <div class="page-content">
        <slot :page="currentPage">
          <h2>{{ currentPage.title }}</h2>
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
import { inject, computed } from 'vue'

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
</style>
