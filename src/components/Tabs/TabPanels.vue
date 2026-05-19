<template>
  <div class="tabs-panels">
    <div
      v-for="tab in tabs"
      :key="tab.id"
      :id="`panel-${tab.id}`"
      role="tabpanel"
      :aria-labelledby="`tab-${tab.id}`"
      :class="['tab-panel', { active: activeTabId === tab.id }]"
    >
      <slot :name="`panel-${tab.id}`" :tab="tab">
        {{ tab.content }}
      </slot>
    </div>
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

const tabsState = inject<TabsState>('tabsState')!

const tabs = computed(() => tabsState.tabs)
const activeTabId = computed(() => tabsState.activeTabId.value)
</script>

<style scoped>
.tabs-panels {
  position: relative;
}

.tab-panel {
  display: none;
  padding: 20px;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-top: none;
  animation: fadeIn 0.2s ease-in;
}

.tab-panel.active {
  display: block;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
