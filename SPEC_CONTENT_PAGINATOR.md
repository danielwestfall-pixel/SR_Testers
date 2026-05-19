# Component Specification: Content Paginator

**Status:** Development Ready  
**Complexity:** Medium-High  
**Accessibility Level:** WCAG 2.1 AA

---

## Purpose

A fully accessible pagination component that displays content across multiple pages with Previous/Next navigation, keyboard support (arrow keys, Home/End), and automatic focus management with live region announcements for screen reader users.

---

## Component Requirements

### Technical Stack
- Vue 3 (Composition API with `<script setup>`)
- TypeScript (recommended)
- CSS for styling (scoped or BEM classes)

### File Structure
```
src/components/
├── ContentPaginator/
│   ├── PaginatedContent.vue
│   └── PaginatedContent.css (or scoped styles)
```

---

## Type Definitions

```typescript
export interface PageContent {
  title?: string
  content: string
}
```

---

## Component: PaginatedContent

### Props

```typescript
interface Props {
  pages: string[] | PageContent[]  // REQUIRED: Array of page content or objects
  itemsPerPage?: number             // Items per page (default: 1)
}

const props = withDefaults(defineProps<Props>(), {
  itemsPerPage: 1
})
```

### State

```typescript
const currentPage = ref(1)

const totalPages = computed(() => 
  Math.ceil(props.pages.length / props.itemsPerPage)
)

const currentPageContent = computed(() => {
  const startIndex = (currentPage.value - 1) * props.itemsPerPage
  const endIndex = startIndex + props.itemsPerPage
  return props.pages.slice(startIndex, endIndex)
})

const currentPageNumber = computed(() => currentPage.value)
```

### Event Handlers

```typescript
const emit = defineEmits<{
  'page-change': [page: number]
}>()

const previousPage = () => {
  if (currentPage.value > 1) {
    goToPage(currentPage.value - 1)
  }
}

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    goToPage(currentPage.value + 1)
  }
}

const goToPage = (page: number) => {
  const validPage = Math.max(1, Math.min(page, totalPages.value))
  currentPage.value = validPage
  emit('page-change', validPage)
  
  // Focus management
  nextTick(() => {
    const titleElement = document.getElementById(`page-title-${validPage}`)
    titleElement?.focus()
  })
}

const handleKeydown = (e: KeyboardEvent) => {
  switch (e.key) {
    case 'ArrowLeft':
      if (currentPage.value > 1) {
        previousPage()
        e.preventDefault()
      }
      break
    case 'ArrowRight':
      if (currentPage.value < totalPages.value) {
        nextPage()
        e.preventDefault()
      }
      break
    case 'Home':
      goToPage(1)
      e.preventDefault()
      break
    case 'End':
      goToPage(totalPages.value)
      e.preventDefault()
      break
  }
}
```

### HTML Structure

```html
<template>
  <div class="paginator-wrapper" @keydown="handleKeydown">
    <!-- Page content area -->
    <div 
      class="page-content" 
      role="main" 
      aria-live="polite"
      aria-label="Page content"
    >
      <h3 :id="`page-title-${currentPageNumber}`" tabindex="-1">
        {{ getPageTitle() }}
      </h3>
      
      <div class="page-body">
        <div v-for="(page, index) in currentPageContent" :key="index">
          {{ typeof page === 'string' ? page : page.content }}
        </div>
      </div>
    </div>

    <!-- Page indicator and navigation -->
    <div 
      class="paginator-controls" 
      role="navigation" 
      aria-label="Pagination navigation"
    >
      <!-- Previous button -->
      <button
        @click="previousPage"
        :disabled="currentPage === 1"
        :aria-label="`Previous page, page ${currentPage - 1}`"
        class="nav-button prev-button"
      >
        ← Previous
      </button>

      <!-- Page indicator -->
      <span class="page-info" aria-live="polite" aria-atomic="true">
        Page <strong>{{ currentPage }}</strong> of {{ totalPages }}
      </span>

      <!-- Next button -->
      <button
        @click="nextPage"
        :disabled="currentPage === totalPages"
        :aria-label="`Next page, page ${currentPage + 1}`"
        class="nav-button next-button"
      >
        Next →
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
const getPageTitle = () => {
  const page = currentPageContent.value[0]
  if (typeof page === 'object' && page.title) {
    return page.title
  }
  return `Page ${currentPage.value} of ${totalPages.value}`
}
</script>
```

---

## Accessibility Requirements (a11y)

### ✅ ARIA Attributes

| Element | Attribute | Value | Purpose | Notes |
|---------|-----------|-------|---------|-------|
| Page Content | `role` | `"main"` | Main content area | Semantic landmark |
| Page Content | `aria-live` | `"polite"` | Announce content changes | Wait for natural pause |
| Page Content | `aria-label` | `"Page content"` | Describes region | Optional but recommended |
| Page Title | `tabindex` | `"-1"` | Focusable via JS only | Needed for focus management |
| Page Title | `id` | `"page-title-{n}"` | Reference for focus | Must be unique per page |
| Controls | `role` | `"navigation"` | Navigation landmark | Semantic meaning |
| Controls | `aria-label` | `"Pagination navigation"` | Describes nav purpose | Distinguishes from other nav |
| Page Info | `aria-live` | `"polite"` | Announce page changes | Update when page changes |
| Page Info | `aria-atomic` | `"true"` | Read entire message | Important for screen readers |
| Prev Button | `aria-label` | `"Previous page, page {n}"` | Descriptive label | Includes destination |
| Next Button | `aria-label` | `"Next page, page {n}"` | Descriptive label | Includes destination |

### ✅ Semantic HTML

- **Use `<button>` elements for Previous/Next**, not `<div>`
- **Use `<div role="navigation">` or `<nav>` for controls wrapper**
- **Use `<main>` or `<div role="main">` for content area**
- **Page title should be `<h3>` or appropriate heading level**

### ✅ Keyboard Navigation

| Key(s) | Behavior | Required? |
|--------|----------|-----------|
| Tab | Focus Previous/Next buttons | Yes |
| Shift+Tab | Move focus backward | Yes |
| Enter/Space | Activate focused button | Yes (native `<button>`) |
| Left Arrow | Go to previous page | Yes |
| Right Arrow | Go to next page | Yes |
| Home | Go to first page | Yes |
| End | Go to last page | Yes |

**Important:**
- Arrow keys should prevent default to avoid page scrolling
- Buttons at boundaries should be disabled (Previous on page 1, Next on last page)
- Disabled buttons should announce their state
- Focus should move to page title when page changes

### ✅ Screen Reader Announcements

| Situation | Screen Reader Output |
|-----------|----------------------|
| Page 1 of 5 loads | "Page content region. Page title. [content]" |
| User presses Right arrow | (aria-live polite) "Page 2 of 5" |
| Focus moves to title | "Page title. Page 2 of 5" |
| Previous button disabled | "Previous page, page 0, button, disabled" |
| Next button active | "Next page, page 3, button" |

### ✅ Focus Management

**Critical:** Focus must move to page title when page changes

```typescript
watch(currentPage, () => {
  nextTick(() => {
    document.getElementById(`page-title-${currentPage.value}`)?.focus()
  })
})
```

- **Page title should have `tabindex="-1"`** so it's focusable via JavaScript but not by Tab
- **Title element should have unique ID** (page-title-1, page-title-2, etc.)
- **Focus change announces the title to screen reader users**
- **Keyboard users know where they are after page change**

### ✅ Live Region Updates

```html
<!-- Page indicator announces changes -->
<span class="page-info" aria-live="polite" aria-atomic="true">
  Page <strong>{{ currentPage }}</strong> of {{ totalPages }}
</span>
```

- `aria-live="polite"` - Wait for natural pause in screen reader speech
- `aria-atomic="true"` - Always read entire "Page X of Y" message
- **Updated automatically when currentPage changes**
- **Users immediately know which page they're on**

### ✅ Button States

**Disabled State:**
```html
<button
  @click="previousPage"
  :disabled="currentPage === 1"
  :aria-label="`Previous page, page ${currentPage - 1}`"
>
  ← Previous
</button>
```

- First page: Previous button is disabled
- Last page: Next button is disabled
- Screen readers announce "disabled" state
- Disabled buttons should appear visually different (grayed out, reduced opacity)
- Disabled buttons should be keyboard inaccessible

**Aria Labels Include Destination:**
- "Previous page, page 2" (not just "Previous")
- "Next page, page 4" (not just "Next")
- Users know where the button will take them

### ✅ Visual States

```css
.nav-button {
  /* Active button */
  background-color: #0066cc;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 200ms ease;
}

.nav-button:hover:not(:disabled) {
  /* Hover state */
  background-color: #0052a3;
}

.nav-button:focus {
  /* Focus indicator */
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.nav-button:disabled {
  /* Disabled state */
  background-color: #ccc;
  cursor: not-allowed;
  opacity: 0.5;
}

.page-info {
  /* Page indicator */
  font-size: 14px;
  color: #666;
  font-weight: normal;
}

.page-info strong {
  /* Current page number emphasized */
  font-weight: bold;
  color: #333;
}

.page-content {
  /* Content area */
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #eee;
  border-radius: 4px;
  min-height: 200px;
}

.page-content h3 {
  /* Page title */
  margin-top: 0;
  color: #333;
}
```

### ✅ Color Contrast

- Button text color ≥ 4.5:1 contrast with background
- Page indicator text ≥ 4.5:1 contrast
- Disabled button must still be distinguishable
- Don't rely on color alone to indicate disabled state

### ✅ Focus Indicators

- **Minimum 2px outline** for WCAG AA
- **Outline offset of 2px** (outside the button)
- **Visible on all interactive elements**
- **Sufficient contrast** between outline and background

### ✅ Touch Targets

- Minimum 44x44 pixel hit area for buttons
- Adequate padding and spacing
- Mobile-friendly interaction area

---

## Styling Requirements

### CSS Classes & Structure

```css
.paginator-wrapper {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.page-content {
  flex: 1;
  padding: 20px;
  background-color: #f9f9f9;
  border: 1px solid #eee;
  border-radius: 4px;
  min-height: 200px;
}

.page-content h3 {
  margin-top: 0;
  color: #333;
}

.page-body {
  line-height: 1.6;
  color: #555;
}

.paginator-controls {
  display: flex;
  align-items: center;
  gap: 15px;
  justify-content: center;
  flex-wrap: wrap;
}

.nav-button {
  padding: 8px 16px;
  background-color: #0066cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 200ms ease;
}

.nav-button:hover:not(:disabled) {
  background-color: #0052a3;
}

.nav-button:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.nav-button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.nav-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
  opacity: 0.5;
}

.prev-button {
  order: 1;
}

.page-info {
  order: 2;
  font-size: 14px;
  color: #666;
  min-width: 150px;
  text-align: center;
}

.page-info strong {
  font-weight: bold;
  color: #333;
}

.next-button {
  order: 3;
}

/* Mobile responsive */
@media (max-width: 600px) {
  .paginator-wrapper {
    padding: 15px;
    gap: 15px;
  }

  .paginator-controls {
    gap: 10px;
  }

  .nav-button {
    padding: 6px 12px;
    font-size: 13px;
  }

  .page-info {
    min-width: 120px;
  }
}
```

---

## Testing Checklist

### ✅ Keyboard Testing
- [ ] Can Tab to Previous button
- [ ] Can Tab to Next button
- [ ] Can Tab away from paginator
- [ ] Can activate Previous with Enter key
- [ ] Can activate Previous with Space key
- [ ] Can activate Next with Enter key
- [ ] Can activate Next with Space key
- [ ] Left arrow goes to previous page
- [ ] Right arrow goes to next page
- [ ] Home key goes to page 1
- [ ] End key goes to last page
- [ ] Arrow keys prevent page scroll
- [ ] Previous button disabled on page 1
- [ ] Next button disabled on last page
- [ ] No keyboard traps

### ✅ Screen Reader Testing (NVDA/JAWS)
- [ ] Navigation region announced
- [ ] Page content announced as main
- [ ] Page title announced with focus
- [ ] Page indicator "Page X of Y" announced when page changes
- [ ] Button aria-labels include destination page number
- [ ] Disabled button state is announced
- [ ] aria-live updates are announced without interrupting
- [ ] Page content is announced when it becomes active
- [ ] Focus moving to title is announced

### ✅ Visual Testing
- [ ] Previous/Next buttons visible and readable
- [ ] Page indicator clearly shows current page
- [ ] Current page number is bold or emphasized
- [ ] Disabled button appears grayed out
- [ ] Hover state provides visual feedback
- [ ] Focus indicator visible on all buttons
- [ ] Page content area clearly distinguished
- [ ] Content updates when page changes
- [ ] Smooth transitions between pages

### ✅ Accessibility Validation
- [ ] Run axe DevTools - no violations
- [ ] Run WAVE - no errors
- [ ] Test with NVDA screen reader
- [ ] Test with JAWS screen reader (if possible)
- [ ] Verify all keyboard interactions
- [ ] Check color contrast (4.5:1 minimum)
- [ ] Test with keyboard only (no mouse)
- [ ] Verify focus indicators on all interactive elements

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
    <h1>Documentation</h1>
    <PaginatedContent 
      :pages="pages"
      :itemsPerPage="1"
      @page-change="onPageChange"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import PaginatedContent from '@/components/ContentPaginator/PaginatedContent.vue'

const pages = ref([
  {
    title: 'Introduction',
    content: 'Welcome to the documentation...'
  },
  {
    title: 'Getting Started',
    content: 'Follow these steps to get started...'
  },
  {
    title: 'Advanced Usage',
    content: 'For advanced users, you can...'
  }
])

const onPageChange = (page: number) => {
  console.log(`Navigated to page ${page}`)
}
</script>
```

---

## Acceptance Criteria

- [ ] Component renders without errors
- [ ] Previous/Next buttons work
- [ ] Buttons disable at page boundaries
- [ ] Page indicator shows correct page number
- [ ] Page content updates when navigating
- [ ] Focus moves to page title after navigation
- [ ] Page title has unique ID for each page
- [ ] Page title is focusable via JavaScript
- [ ] Left/Right arrow keys navigate pages
- [ ] Home key goes to first page
- [ ] End key goes to last page
- [ ] Arrow keys prevent page scroll
- [ ] aria-label includes destination page number
- [ ] aria-live announces page changes
- [ ] Focus indicator visible on all buttons
- [ ] Disabled button state clearly visible
- [ ] Screen reader announces navigation region
- [ ] Screen reader announces page content as main
- [ ] Screen reader announces button states
- [ ] Color contrast ≥ 4.5:1 on all states
- [ ] All a11y tests pass
- [ ] No axe or WAVE violations

---

## Notes for Developer

1. **Focus Management is Key**: The focus must move to the page title (`tabindex="-1"`) after page navigation. This alerts screen reader users and keyboard users that the page has changed.

2. **Live Regions are Polite**: Using `aria-live="polite"` doesn't interrupt screen readers, making the experience less jarring than "assertive" announcements.

3. **Button Boundaries**: Buttons should be disabled at page boundaries (Previous on page 1, Next on last page). This provides clear UX and prevents errors.

4. **Aria Labels Include Context**: "Next page, page 3" is better than just "Next" - users know what will happen when they click.

5. **Arrow Key Prevention**: Call `preventDefault()` on arrow key events to prevent page scrolling while navigating pages.

6. **Content Updates Are Automatic**: The computed property `currentPageContent` automatically updates when `currentPage` changes, triggering the reactive update.

7. **ID Consistency**: Page title IDs must follow a predictable pattern (page-title-1, page-title-2, etc.) for JavaScript focus management to work.

8. **Touch Friendly**: Ensure buttons are at least 44x44 pixels for mobile touch targets.

---

## Related Guidelines

- [WAI-ARIA Pagination Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/pagination/)
- [WCAG 2.1 Success Criterion 2.1.1 - Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard)
- [WCAG 2.1 Success Criterion 3.2.3 - Consistent Navigation](https://www.w3.org/WAI/WCAG21/Understanding/consistent-navigation)
- [WCAG 2.1 Success Criterion 4.1.3 - Status Messages](https://www.w3.org/WAI/WCAG21/Understanding/status-messages)
- [WAI-ARIA Live Regions](https://www.w3.org/WAI/ARIA/apg/patterns/liveregion/)
