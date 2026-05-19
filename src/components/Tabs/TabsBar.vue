<template>
  <div class="tabs-bar" role="tablist" :aria-label="ariaLabel">
    <button
      v-for="tab in tabs"
      :key="tab.id"
      role="tab"
      :id="`tab-${tab.id}`"
      :aria-selected="activeTabId === tab.id"
      :aria-controls="`panel-${tab.id}`"
      :disabled="tab.disabled"
      :class="['tab-button', { active: activeTabId === tab.id, disabled: tab.disabled }]"
      @click="selectTab(tab.id)"
      @keydown="handleKeyDown"
    >
      {{ tab.label }}
    </button>
  </div>
</template>

<script setup lang="ts">
import { inject, computed } from 'vue'

interface TabItem {
  id: string
  label: string
  content: string
  disabled?: boolean
}

interface TabsState {
  tabs: TabItem[]
  activeTabId: { value: string }
  selectTab: (tabId: string) => void
}

interface Props {
  ariaLabel?: string
}

withDefaults(defineProps<Props>(), {
  ariaLabel: 'Tabs'
})

const tabsState = inject<TabsState>('tabsState')!

const tabs = computed(() => tabsState.tabs)
const activeTabId = computed(() => tabsState.activeTabId.value)
const selectTab = tabsState.selectTab

const handleKeyDown = (e: KeyboardEvent) => {
  const target = e.target as HTMLButtonElement
  const tabId = target.id.replace('tab-', '')
  const currentIndex = tabs.value.findIndex(t => t.id === tabId)
  
  let nextIndex: number | null = null
  
  switch (e.key) {
    case 'ArrowRight':
    case 'ArrowDown':
      e.preventDefault()
      nextIndex = currentIndex + 1
      if (nextIndex >= tabs.value.length) {
        nextIndex = 0
      }
      break
    case 'ArrowLeft':
    case 'ArrowUp':
      e.preventDefault()
      nextIndex = currentIndex - 1
      if (nextIndex < 0) {
        nextIndex = tabs.value.length - 1
      }
      break
    case 'Home':
      e.preventDefault()
      nextIndex = 0
      break
    case 'End':
      e.preventDefault()
      nextIndex = tabs.value.length - 1
      break
  }
  
  if (nextIndex !== null) {
    const nextTab = tabs.value[nextIndex]
    selectTab(nextTab.id)
    // Focus the new tab button
    setTimeout(() => {
      document.getElementById(`tab-${nextTab.id}`)?.focus()
    }, 0)
  }
}
</script>

<style scoped>
.tabs-bar {
  display: flex;
  gap: 0;
  border-bottom: 2px solid #e0e0e0;
  background-color: #f5f5f5;
}

.tab-button {
  flex: 1;
  padding: 12px 16px;
  border: none;
  background: transparent;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  color: #666;
  transition: all 0.3s ease;
  border-bottom: 3px solid transparent;
  margin-bottom: -2px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.tab-button:hover:not(.disabled) {
  background-color: #efefef;
  color: #333;
}

.tab-button.active {
  color: #0066cc;
  border-bottom-color: #0066cc;
  background-color: white;
}

.tab-button.disabled {
  opacity: 0.5;
  cursor: not-allowed;
  color: #999;
}

.tab-button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: -2px;
}
</style>
