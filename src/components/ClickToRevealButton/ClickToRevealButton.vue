<template>
  <div class="reveal-container">
    <button
      :aria-expanded="isRevealed"
      :aria-label="ariaLabel"
      :aria-controls="isRevealed ? 'reveal-content' : undefined"
      class="reveal-button"
      :class="{ revealed: isRevealed }"
      @click="toggleRevealed"
    >
      <span v-if="!isRevealed" class="reveal-text">Click to reveal</span>
      <span v-if="isRevealed" class="hidden-text">{{ revealContent }}</span>
    </button>

    <!-- Screen reader announcements -->
    <div
      id="reveal-announcement"
      aria-live="polite"
      aria-atomic="true"
      class="sr-only"
    >
      {{ srAnnouncement }}
    </div>

    <!-- Content reference for accessibility - only in DOM when revealed -->
    <div v-if="isRevealed" id="reveal-content" class="sr-only">
      {{ revealContent }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Props {
  revealContent: string
  revealText?: string
  hideText?: string
}

const props = withDefaults(defineProps<Props>(), {
  revealText: 'Click to reveal',
  hideText: 'Click to hide',
})

const isRevealed = ref(false)

const ariaLabel = computed(() => {
  if (isRevealed.value) {
    return `${props.hideText}: ${props.revealContent}`
  }
  return props.revealText
})

const srAnnouncement = computed(() => {
  if (isRevealed.value) {
    return `Content revealed: ${props.revealContent}`
  }
  return ''
})

const toggleRevealed = () => {
  isRevealed.value = !isRevealed.value
}
</script>

<style scoped>
.reveal-container {
  display: inline-block;
}

.reveal-button {
  padding: 0.75rem 1.25rem;
  font-size: 1rem;
  font-weight: 600;
  border: 2px solid #0066cc;
  border-radius: 4px;
  background-color: #f0f8ff;
  color: #0066cc;
  cursor: pointer;
  transition: all 0.2s ease;
  font-family: inherit;
  min-height: 44px;
  min-width: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.reveal-button:hover:not(:disabled) {
  background-color: #e6f2ff;
  border-color: #0052a3;
}

.reveal-button:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.reveal-button:active:not(:disabled) {
  background-color: #cce5ff;
}

.reveal-button.revealed {
  background-color: #fff3cd;
  border-color: #ff9800;
  color: #333;
}

.reveal-button.revealed:hover:not(:disabled) {
  background-color: #ffe8a8;
  border-color: #f57c00;
}

.reveal-text {
  font-weight: 600;
}

.hidden-text {
  font-weight: normal;
  font-style: italic;
}

/* Screen reader only text - hidden visually but available to assistive technology */
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
