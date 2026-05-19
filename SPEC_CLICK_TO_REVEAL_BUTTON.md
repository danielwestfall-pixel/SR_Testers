# Component Specification: Click to Reveal Button

**Status:** Development Ready  
**Complexity:** Medium  
**Accessibility Level:** WCAG 2.1 AA

---

## Purpose

A fully accessible button component that toggles between hiding and revealing sensitive content (spoilers, answers, etc.) with dynamic ARIA labeling and screen reader announcements.

---

## Component Requirements

### Technical Stack
- Vue 3 (Composition API with `<script setup>`)
- TypeScript (recommended)
- CSS for styling (scoped or BEM classes)

### File Structure
```
src/components/
├── ClickToRevealButton.vue
└── ClickToRevealButton.css (or scoped styles)
```

---

## Props

```typescript
interface Props {
  revealContent: string      // REQUIRED: Text to reveal when button clicked
  revealText?: string        // Default: "Click to reveal"
  hideText?: string          // Default: "Click to hide"
}

const props = withDefaults(defineProps<Props>(), {
  revealText: 'Click to reveal',
  hideText: 'Click to hide',
})
```

---

## State Management

### Reactive State
```javascript
const isRevealed = ref(false)  // Boolean: false = hidden, true = revealed
```

### Computed Properties
```javascript
// Screen reader label that updates with state
const ariaLabel = computed(() => {
  if (isRevealed.value) {
    return `${props.hideText}: ${props.revealContent}`
  }
  return props.revealText
})

// Live region announcement content
const srAnnouncement = computed(() => {
  if (isRevealed.value) {
    return `Content revealed: ${props.revealContent}`
  }
  return ''
})
```

### Event Handler
```javascript
const toggleRevealed = () => {
  isRevealed.value = !isRevealed.value
}
```

---

## HTML Structure

### Required Elements

```html
<template>
  <!-- Main button container -->
  <button
    :aria-expanded="isRevealed"
    :aria-label="ariaLabel"
    :aria-controls="isRevealed ? 'reveal-content' : undefined"
    class="reveal-button"
    :class="{ revealed: isRevealed }"
    @click="toggleRevealed"
  >
    <!-- Button text - only one shows at a time -->
    <span v-if="!isRevealed" class="reveal-text">{{ revealText }}</span>
    <span v-if="isRevealed" class="hidden-text">{{ revealContent }}</span>
  </button>

  <!-- Live region for screen reader announcements -->
  <div
    id="spoiler-announcement"
    aria-live="polite"
    aria-atomic="true"
    class="sr-only"
  >
    {{ srAnnouncement }}
  </div>

  <!-- Content reference (only in DOM when revealed) -->
  <div v-if="isRevealed" id="reveal-content" class="sr-only">
    {{ revealContent }}
  </div>
</template>
```

---

## Accessibility Requirements (a11y)

### ✅ ARIA Attributes

| Attribute | Value | Purpose | Notes |
|-----------|-------|---------|-------|
| `aria-expanded` | `"true"` \| `"false"` | Indicates if button controls expanded/collapsed content | Must reflect `isRevealed` state |
| `aria-label` | Dynamic string | Accessible name for button | Must change with state |
| `aria-controls` | `"reveal-content"` or undefined | Links button to controlled content | Only set when content exists in DOM |
| `aria-live` | `"polite"` | Announces changes without interrupting | On live region div |
| `aria-atomic` | `"true"` | Read entire announcement message | On live region div |

### ✅ Semantic HTML

- **Must use `<button>` element**, not `<div>` or `<span>`
  - Provides native keyboard support (Tab, Enter, Space)
  - Screen readers announce as "button"
  - Standard focus management

### ✅ Keyboard Navigation

| Key(s) | Behavior | Required? |
|--------|----------|-----------|
| Tab | Focus button, move to next element | Yes (native) |
| Shift+Tab | Move focus to previous element | Yes (native) |
| Enter | Toggle reveal state | Yes (native) |
| Space | Toggle reveal state | Yes (native) |

**Implementation:** No additional code needed - native `<button>` handles this automatically.

### ✅ Screen Reader Announcements

| State | Screen Reader Output |
|-------|----------------------|
| Hidden (initial) | "[revealText], button, collapsed" |
| Revealed (after click) | "Content revealed: [content]" (via aria-live) |
| Button aria-label | Announced when button receives focus |

### ✅ Focus Management

- Focus must always be visible on button
- Focus should remain on button after click (don't shift focus)
- Screen readers announce aria-expanded when button is focused

### ✅ Content Availability

**Critical:** Content must ONLY exist in DOM when revealed
- `v-if="isRevealed"` on the reveal-content div
- Screen readers cannot access what's not in the DOM
- aria-controls reference is only valid when content exists
- Prevents broken ARIA references

### ✅ Visual States

Provide clear visual indicators for both states:

```css
.reveal-button {
  /* Hidden state */
  background-color: #f0f8ff;
  border: 2px solid #0066cc;
  color: #0066cc;
  /* ... other styles ... */
}

.reveal-button.revealed {
  /* Revealed state - visually distinct */
  background-color: #fff3cd;
  border-color: #ff9800;
  color: #333;
}

.reveal-button:focus {
  /* Focus indicator - 2px outline minimum for WCAG AA */
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
```

### ✅ Color Contrast

- Text color and background must have contrast ratio ≥ 4.5:1 (WCAG AA)
- Both hidden and revealed states must meet this requirement
- Don't rely on color alone to indicate state

### ✅ Screen Reader Only Class

```css
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
```

**Important:** Not `display: none` or `visibility: hidden` - those hide from screen readers too.

---

## Styling Requirements

### CSS Classes

```css
/* Button in hidden state */
.reveal-button {
  display: inline-block;
  padding: 10px 16px;
  font-size: 16px;
  font-weight: 500;
  border: 2px solid;
  border-radius: 4px;
  cursor: pointer;
  transition: all 200ms ease;
  background-color: #f0f8ff;
  border-color: #0066cc;
  color: #0066cc;
}

/* Button in revealed state */
.reveal-button.revealed {
  background-color: #fff3cd;
  border-color: #ff9800;
  color: #333;
}

/* Hover state (mouse users) */
.reveal-button:hover:not(:disabled) {
  background-color: #e6f2ff;
  border-color: #0052a3;
}

/* Focus state (keyboard users) */
.reveal-button:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Focus within the button (for browser support) */
.reveal-button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Disabled state (if implemented) */
.reveal-button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

/* Text spans */
.reveal-text {
  /* "Click to reveal" text */
}

.hidden-text {
  /* Revealed content text */
}

/* Live region (visually hidden) */
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
```

### Touch Targets

- Minimum 44x44 pixel hit area (WCAG best practice)
- Sufficient padding on button for adequate touch size

---

## Testing Checklist

### ✅ Keyboard Testing
- [ ] Can Tab to button
- [ ] Button receives focus indicator
- [ ] Can press Enter to toggle
- [ ] Can press Space to toggle
- [ ] Can Shift+Tab to move focus away
- [ ] No keyboard traps

### ✅ Screen Reader Testing (NVDA/JAWS)
- [ ] Button announces name correctly
- [ ] aria-expanded state is announced
- [ ] Button announces as "collapsed" when hidden
- [ ] Button announces as "expanded" when revealed
- [ ] aria-label includes both hide text and content when revealed
- [ ] Live region announces "Content revealed: [content]" when revealed
- [ ] Content is NOT announced when hidden
- [ ] aria-controls reference is valid when content exists

### ✅ Visual Testing
- [ ] Button is visually distinct in both states
- [ ] Focus indicator is clearly visible
- [ ] Hover state provides visual feedback
- [ ] Text displays correctly in both states
- [ ] Color contrast meets WCAG AA (4.5:1)

### ✅ Accessibility Validation
- [ ] Run axe DevTools - no violations
- [ ] Run WAVE - no errors
- [ ] Test with NVDA screen reader
- [ ] Test with JAWS screen reader (if possible)
- [ ] Test all keyboard interactions
- [ ] Verify mobile accessibility (if applicable)

### ✅ Browser Testing
- [ ] Chrome/Chromium
- [ ] Firefox
- [ ] Safari
- [ ] Edge

---

## Usage Example

```vue
<template>
  <ClickToRevealButton 
    :revealContent="spoilerContent"
    revealText="Show me the spoiler"
    hideText="Hide the spoiler"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import ClickToRevealButton from '@/components/ClickToRevealButton.vue'

const spoilerContent = ref('The cake is a lie')
</script>
```

---

## Acceptance Criteria

- [ ] Component renders without errors
- [ ] Button toggles revealed state on click
- [ ] Button text changes between reveal and hide states
- [ ] aria-expanded updates correctly
- [ ] aria-label updates with state and content
- [ ] Live region announces content reveal
- [ ] Focus is visible when tabbing to button
- [ ] All keyboard interactions work (Enter, Space)
- [ ] Screen reader announces state changes
- [ ] aria-controls only set when content exists in DOM
- [ ] Content only in DOM when revealed
- [ ] Visual styling matches both states
- [ ] Color contrast ≥ 4.5:1
- [ ] All a11y tests pass
- [ ] No axe or WAVE violations

---

## Notes for Developer

1. **Content Availability is Critical**: The reveal-content div must be removed from DOM entirely when hidden. This is not just a visual hiding - screen readers literally cannot see it if it's not there.

2. **Conditional aria-controls**: The aria-controls attribute must be undefined when content doesn't exist. Pointing to non-existent elements breaks ARIA semantics.

3. **Live Region Polite**: Use "polite" not "assertive" for announcements - this doesn't interrupt screen reader users mid-sentence.

4. **No Custom Keyboard Handling Needed**: Because we use a native `<button>`, Enter/Space/Tab all work automatically.

5. **Focus Management**: Don't move focus away from button after click - this is disorienting for keyboard users.

6. **Computed Properties**: Use computed properties for ariaLabel and srAnnouncement - they're efficient and automatically update when isRevealed changes.

---

## Related Guidelines

- [WAI-ARIA Button Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/)
- [WCAG 2.1 Success Criterion 4.1.2 - Name, Role, Value](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value)
- [WCAG 2.1 Success Criterion 2.1.1 - Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard)
- [WCAG 2.1 Success Criterion 3.2.4 - Consistent Identification](https://www.w3.org/WAI/WCAG21/Understanding/consistent-identification)
