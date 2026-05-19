# Component Specification: Tabs Component

**Status:** Development Ready  
**Complexity:** High  
**Accessibility Level:** WCAG 2.1 AA

---

## Purpose

A fully accessible tabbed interface component that displays content across multiple tabs with keyboard navigation (arrow keys, Home/End) and complete ARIA roles and properties for screen reader compatibility.

---

## Component Requirements

### Technical Stack
- Vue 3 (Composition API with `<script setup>`)
- TypeScript (recommended)
- CSS for styling (scoped or BEM classes)

### File Structure
```
src/components/
├── Tabs/
│   ├── TabsContainer.vue
│   ├── TabsBar.vue
│   ├── TabPanels.vue
│   ├── useTabs.ts (composable for shared logic)
│   └── Tabs.css (or scoped styles)
└── types/
   └── tabs.ts (type definitions)
```

---

## Type Definitions

```typescript
// types/tabs.ts
export interface TabItem {
  id: string        // Unique identifier (e.g., "tab-1")
  label: string     // Display text for tab button
  content: string   // Content to display in panel
}
```

---

## Component 1: TabsContainer (Root/State Manager)

### Props
```typescript
interface Props {
  items: TabItem[]            // REQUIRED: Array of tabs
  initialTabId?: string       // Which tab starts active (default: first)
}

const props = withDefaults(defineProps<Props>(), {
  initialTabId: () => ''
})
```

### State
```typescript
const activeTabId = ref(props.initialTabId || props.items[0]?.id || '')

const selectTab = (tabId: string) => {
  if (props.items.some(item => item.id === tabId)) {
    activeTabId.value = tabId
  }
}
```

### HTML Structure
```html
<template>
  <div class="tabs-container">
    <TabsBar 
      :items="items" 
      :activeTabId="activeTabId" 
      @select="selectTab" 
    />
    <TabPanels 
      :items="items" 
      :activeTabId="activeTabId" 
    />
  </div>
</template>
```

---

## Component 2: TabsBar (Tab Buttons)

### Props
```typescript
interface Props {
  items: TabItem[]
  activeTabId: string
}

const props = defineProps<Props>()

const emit = defineEmits<{
  select: [tabId: string]
}>()
```

### Event Handlers
```typescript
const handleSelect = (tabId: string) => {
  emit('select', tabId)
}

const handleKeydown = (e: KeyboardEvent, index: number) => {
  let newIndex = index
  
  switch (e.key) {
    case 'ArrowLeft':
      newIndex = (index - 1 + props.items.length) % props.items.length
      e.preventDefault()
      break
    case 'ArrowRight':
      newIndex = (index + 1) % props.items.length
      e.preventDefault()
      break
    case 'Home':
      newIndex = 0
      e.preventDefault()
      break
    case 'End':
      newIndex = props.items.length - 1
      e.preventDefault()
      break
    default:
      return
  }
  
  const tabId = props.items[newIndex]?.id
  emit('select', tabId)
  
  // Focus the newly selected tab
  nextTick(() => {
    document.getElementById(`tab-${tabId}`)?.focus()
  })
}
```

### HTML Structure
```html
<template>
  <div role="tablist" class="tabs-bar">
    <button
      v-for="(item, index) in items"
      :key="item.id"
      :id="`tab-${item.id}`"
      role="tab"
      :aria-selected="activeTabId === item.id"
      :aria-controls="`panel-${item.id}`"
      class="tab-button"
      :class="{ active: activeTabId === item.id }"
      @click="handleSelect(item.id)"
      @keydown="handleKeydown($event, index)"
    >
      {{ item.label }}
    </button>
  </div>
</template>
```

---

## Component 3: TabPanels (Tab Content)

### Props
```typescript
interface Props {
  items: TabItem[]
  activeTabId: string
}

const props = defineProps<Props>()
```

### HTML Structure
```html
<template>
  <div class="tab-panels">
    <div
      v-for="item in items"
      :key="item.id"
      :id="`panel-${item.id}`"
      role="tabpanel"
      :aria-labelledby="`tab-${item.id}`"
      :hidden="activeTabId !== item.id"
      class="tab-panel"
      :class="{ active: activeTabId === item.id }"
    >
      {{ item.content }}
    </div>
  </div>
</template>
```

---

## Accessibility Requirements (a11y)

### ✅ ARIA Attributes

| Element | Attribute | Value | Purpose | Notes |
|---------|-----------|-------|---------|-------|
| TabsBar | `role` | `"tablist"` | Container role | Required |
| Tab Button | `role` | `"tab"` | Button role | Required |
| Tab Button | `aria-selected` | `"true"` or `"false"` | Active tab indicator | Must update with state |
| Tab Button | `aria-controls` | `"panel-{id}"` | Links to controlled panel | ID must exist |
| Panel | `role` | `"tabpanel"` | Panel role | Required |
| Panel | `aria-labelledby` | `"tab-{id}"` | Links to labeling tab | ID must exist |
| Panel | `hidden` | Boolean attribute | Visual/semantic hiding | `hidden="hidden"` or `:hidden="true"` |

### ✅ Semantic HTML

- **Tabs must be `<button>` elements**, not `<div>`
- **Tablist must be `<div role="tablist">` or `<nav>`**
- **Panels must be `<div role="tabpanel">`**

### ✅ Keyboard Navigation

| Key(s) | Behavior | Required? |
|--------|----------|-----------|
| Tab | Move focus between interactive elements | Yes |
| Shift+Tab | Move focus to previous interactive element | Yes |
| Arrow Left | Select previous tab | Yes |
| Arrow Right | Select next tab | Yes |
| Home | Select first tab | Yes |
| End | Select last tab | Yes |

**Important:** 
- Arrow keys should wrap (Left on first tab goes to last tab)
- Arrow keys must call `preventDefault()` so page doesn't scroll
- Arrow key focus should move AND select the tab
- Focus must move to the newly selected tab button

### ✅ Screen Reader Announcements

| Situation | Screen Reader Output |
|-----------|----------------------|
| Focus tab button | "[label], tab, [number] of [total], selected/not selected" |
| Active tab changes | Tab is announced as selected when it receives focus |
| Panel becomes visible | Panel role announced with tab label via aria-labelledby |

### ✅ Focus Management

- **Focus indicator must always be visible** on active tab button
- **Focus must move to newly selected tab** when using keyboard
- **Only one tab should be in tab order** at a time (others have tabindex="-1" is optional but recommended)
- **Focus should land on tab button, not the panel content**

### ✅ Tab Selection Logic

- **Only one tab can be selected at a time**
- **aria-selected="true"` on active tab, `"false"` on others**
- **Visual state must match aria-selected state**
- **Clicking or keyboard selecting a tab shows its panel**

### ✅ Panel Visibility

```html
<!-- When not active: -->
<div role="tabpanel" aria-labelledby="tab-1" hidden>
  Content hidden from visual and screen reader users
</div>

<!-- When active: -->
<div role="tabpanel" aria-labelledby="tab-1">
  Content visible to both visual and screen reader users
</div>
```

**Important:** Use `hidden` attribute, not `display: none` in CSS
- The `hidden` attribute is semantic and understood by all browsers
- Screen readers respect it
- Keyboard focus doesn't go to hidden content

### ✅ Visual States

```css
.tab-button {
  /* Inactive tab */
  background-color: #f5f5f5;
  border-bottom: 2px solid transparent;
  color: #666;
  border: none;
  padding: 12px 16px;
  cursor: pointer;
  font-size: 16px;
}

.tab-button.active {
  /* Active tab - must be visually distinct */
  background-color: #fff;
  border-bottom-color: #0066cc;
  color: #0066cc;
  font-weight: bold;
}

.tab-button:hover:not(:disabled) {
  /* Hover state for mouse users */
  background-color: #efefef;
}

.tab-button:focus {
  /* Focus indicator for keyboard users */
  outline: 2px solid #0066cc;
  outline-offset: -2px;  /* Or use outline-offset: 2px */
}

.tab-panel {
  padding: 20px;
  border-top: 2px solid #0066cc;
}

.tab-panel[hidden] {
  display: none;
}
```

### ✅ Color Contrast

- Active tab label: contrast ≥ 4.5:1 (WCAG AA)
- Inactive tab label: contrast ≥ 4.5:1 (WCAG AA)
- Border color: visible against background
- Don't rely on color alone to show active state

### ✅ Disabled Tabs (Optional)

If implementing disabled tabs:

```html
<button
  role="tab"
  :disabled="item.disabled"
  :aria-disabled="item.disabled"
  @click="item.disabled ? null : handleSelect(item.id)"
  @keydown="item.disabled ? null : handleKeydown($event, index)"
>
  {{ item.label }}
</button>
```

---

## Styling Requirements

### CSS Classes & Structure

```css
.tabs-container {
  /* Container for entire tabs component */
}

.tabs-bar {
  /* Tab button row */
  display: flex;
  flex-wrap: wrap;
  border-bottom: 1px solid #ddd;
  margin: 0;
  padding: 0;
  list-style: none;
}

.tab-button {
  /* Individual tab button */
  background-color: #f5f5f5;
  border: none;
  border-bottom: 2px solid transparent;
  color: #666;
  padding: 12px 16px;
  font-size: 16px;
  font-weight: normal;
  cursor: pointer;
  transition: all 200ms ease;
  margin: 0;
}

.tab-button:hover:not(:disabled) {
  background-color: #efefef;
}

.tab-button:focus {
  outline: 2px solid #0066cc;
  outline-offset: -2px;
}

.tab-button[aria-selected="true"] {
  background-color: #fff;
  border-bottom-color: #0066cc;
  color: #0066cc;
  font-weight: bold;
}

.tab-panels {
  /* Container for all panels */
}

.tab-panel {
  display: none;
  padding: 20px;
  border-top: 2px solid #0066cc;
}

.tab-panel:not([hidden]) {
  display: block;
}

.tab-panel[hidden] {
  display: none;
}
```

### Responsive Considerations

- Tabs should wrap on small screens
- Consider vertical tab layout on mobile (optional)
- Touch targets should be ≥ 44x44 pixels

---

## Testing Checklist

### ✅ Keyboard Testing
- [ ] Tab to first tab button
- [ ] Tab moves focus between tabs in tablist
- [ ] Tab to move focus out of tablist (wraps around)
- [ ] Shift+Tab to move backward through tabs
- [ ] Left arrow selects previous tab (wraps from first to last)
- [ ] Right arrow selects next tab (wraps from last to first)
- [ ] Home key selects first tab
- [ ] End key selects last tab
- [ ] Keyboard focus moves to newly selected tab
- [ ] Tab clicks also work correctly
- [ ] No keyboard traps

### ✅ Screen Reader Testing (NVDA/JAWS)
- [ ] Tablist announces as "tablist"
- [ ] Tab buttons announce as "tab [number] of [total]"
- [ ] aria-selected state is announced ("selected" or "not selected")
- [ ] Changing tabs updates aria-selected announcements
- [ ] Active tab announces correctly when focused
- [ ] Panel announces with role "tabpanel"
- [ ] Panel is labeled by aria-labelledby (tab label is repeated)
- [ ] Hidden panels are not accessible to screen readers
- [ ] Content inside active panel is readable

### ✅ Visual Testing
- [ ] Active tab visually distinct from inactive tabs
- [ ] Active tab has different color, underline, or background
- [ ] Hover state provides visual feedback
- [ ] Focus indicator visible on all tabs
- [ ] Panel content displays correctly
- [ ] Only one panel visible at a time
- [ ] Transition between panels is smooth

### ✅ Accessibility Validation
- [ ] Run axe DevTools - no violations
- [ ] Run WAVE - no errors
- [ ] Test with NVDA screen reader
- [ ] Test with JAWS screen reader
- [ ] Verify all keyboard interactions
- [ ] Check color contrast (4.5:1 minimum)

### ✅ Browser Testing
- [ ] Chrome/Chromium
- [ ] Firefox
- [ ] Safari
- [ ] Edge

---

## Usage Example

```vue
<template>
  <div>
    <h2>Documentation</h2>
    <TabsContainer :items="tabs" initialTabId="overview">
      <TabsBar :items="tabs" :activeTabId="activeTab" @select="activeTab = $event" />
      <TabPanels :items="tabs" :activeTabId="activeTab" />
    </TabsContainer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import TabsContainer from '@/components/Tabs/TabsContainer.vue'
import TabsBar from '@/components/Tabs/TabsBar.vue'
import TabPanels from '@/components/Tabs/TabPanels.vue'
import type { TabItem } from '@/types/tabs'

const activeTab = ref('overview')

const tabs: TabItem[] = [
  {
    id: 'overview',
    label: 'Overview',
    content: 'This is the overview tab content...'
  },
  {
    id: 'details',
    label: 'Details',
    content: 'This is the details tab content...'
  },
  {
    id: 'settings',
    label: 'Settings',
    content: 'This is the settings tab content...'
  }
]
</script>
```

---

## Acceptance Criteria

- [ ] Component renders without errors
- [ ] All three sub-components work together
- [ ] Clicking tab selects it and shows panel
- [ ] Only one tab can be active at a time
- [ ] aria-selected updates correctly
- [ ] aria-controls and aria-labelledby reference valid IDs
- [ ] Focus indicator visible on all tabs
- [ ] Left/Right arrow keys navigate between tabs
- [ ] Home/End keys jump to first/last tab
- [ ] Arrow keys prevent default (no page scroll)
- [ ] Focus moves to newly selected tab
- [ ] Tabs wrap (left on first goes to last)
- [ ] Screen reader announces tab count and position
- [ ] Screen reader announces aria-selected state
- [ ] Panels only visible when active
- [ ] Hidden panels inaccessible to screen readers
- [ ] Color contrast ≥ 4.5:1 on all states
- [ ] All a11y tests pass
- [ ] No axe or WAVE violations

---

## Notes for Developer

1. **Three Component Pattern**: TabsContainer manages state, TabsBar handles buttons with keyboard, TabPanels displays content. This separation of concerns makes testing and maintenance easier.

2. **Keyboard Navigation is Complex**: The arrow key handling with wrapping and focus management requires careful implementation. Test thoroughly with keyboard users.

3. **aria-selected vs Hidden**: The `hidden` attribute on panels makes them invisible to screen readers. This is semantic and preferred over hiding with CSS.

4. **Focus Management Timing**: Use `nextTick()` to ensure the DOM has updated before trying to focus the newly selected tab.

5. **One Tab in Tab Order**: Consider adding `tabindex="0"` only to the active tab and `tabindex="-1"` to others. This reduces how many Tab presses are needed to navigate out of the tablist.

6. **ID Generation**: All tab IDs should be unique and predictable. Use the TabItem `id` property consistently.

7. **Type Safety**: Use TypeScript interfaces for TabItem to ensure consistency across components.

---

## Related Guidelines

- [WAI-ARIA Tabs Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)
- [WCAG 2.1 Success Criterion 2.1.1 - Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard)
- [WCAG 2.1 Success Criterion 3.2.3 - Consistent Navigation](https://www.w3.org/WAI/WCAG21/Understanding/consistent-navigation)
- [WCAG 2.1 Success Criterion 4.1.2 - Name, Role, Value](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value)
