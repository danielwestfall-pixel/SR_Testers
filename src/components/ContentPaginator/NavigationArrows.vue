<template>
  <div class="navigation-arrows">
    <button
      class="arrow-button prev-button"
      :disabled="!canGoPrev()"
      aria-label="Previous page"
      @click="goPrev"
      @keydown.enter="goPrev"
      @keydown.space.prevent="goPrev"
    >
      <span class="arrow" aria-hidden="true">‹</span>
    </button>
    <button
      class="arrow-button next-button"
      :disabled="!canGoNext()"
      aria-label="Next page"
      @click="goNext"
      @keydown.enter="goNext"
      @keydown.space.prevent="goNext"
    >
      <span class="arrow" aria-hidden="true">›</span>
    </button>
  </div>
</template>

<script setup lang="ts">
import { inject } from 'vue'

interface PaginatorState {
  goNext: () => void
  goPrev: () => void
  canGoNext: () => boolean
  canGoPrev: () => boolean
}

const paginatorState = inject<PaginatorState>('paginatorState')!

const goNext = paginatorState.goNext
const goPrev = paginatorState.goPrev
const canGoNext = paginatorState.canGoNext
const canGoPrev = paginatorState.canGoPrev
</script>

<style scoped>
.navigation-arrows {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  transform: translateY(-50%);
  display: flex;
  justify-content: space-between;
  pointer-events: none;
  padding: 0 20px;
}

.arrow-button {
  position: relative;
  pointer-events: auto;
  width: 60px;
  height: 60px;
  border: none;
  background-color: rgba(0, 102, 204, 0.9);
  color: white;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  font-size: 32px;
  line-height: 1;
  z-index: 10;
}

.arrow-button:hover:not(:disabled) {
  background-color: rgba(0, 102, 204, 1);
  transform: scale(1.1);
}

.arrow-button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 4px;
}

.arrow-button:active:not(:disabled) {
  transform: scale(0.95);
}

.arrow-button:disabled {
  background-color: rgba(200, 200, 200, 0.5);
  cursor: not-allowed;
  opacity: 0.5;
}

.arrow {
  display: block;
  font-weight: bold;
}

.prev-button .arrow {
  margin-right: 2px;
}

.next-button .arrow {
  margin-left: 2px;
}
</style>
