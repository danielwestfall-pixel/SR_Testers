<template>
  <div class="tabs-container">
    <slot />
  </div>
</template>

<script setup lang="ts">
import { ref, provide } from 'vue'

export interface TabItem {
  id: string
  label: string
  content: string
  disabled?: boolean
}

interface Props {
  tabs: TabItem[]
  defaultTabId?: string
}

const props = withDefaults(defineProps<Props>(), {
  defaultTabId: undefined
})

const activeTabId = ref(props.defaultTabId || props.tabs[0]?.id || '')

const selectTab = (tabId: string) => {
  const tab = props.tabs.find(t => t.id === tabId)
  if (tab && !tab.disabled) {
    activeTabId.value = tabId
  }
}

// Provide state to child components
provide('tabsState', {
  tabs: props.tabs,
  activeTabId,
  selectTab
})
</script>

<style scoped>
.tabs-container {
  width: 100%;
}
</style>
