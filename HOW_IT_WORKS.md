# How the Click to Reveal Component Works

A detailed breakdown of the HTML elements, ARIA attributes, Vue state management, and styling that make this accessible component work.

## HTML Structure

### Main Button Element
```html
<button
  :aria-expanded="isRevealed"
  :aria-label="ariaLabel"
  :aria-controls="isRevealed ? 'reveal-content' : undefined"
  class="reveal-button"
  :class="{ revealed: isRevealed }"
  @click="toggleRevealed"
>
```

**What it does:**
- Uses semantic `<button>` element (not a div or span)
- Native keyboard support: Tab to focus, Enter/Space to activate
- Proper focus management and default keyboard behavior
- `aria-controls` is **conditional**: only points to content when it exists in DOM

### Button Content - Two States
```html
<span v-if="!isRevealed" class="reveal-text">Click to reveal</span>
<span v-if="isRevealed" class="hidden-text">{{ revealContent }}</span>
```

**What it does:**
- **First span**: Shows "Click to reveal" when content is hidden (`v-if="!isRevealed"`)
- **Second span**: Shows the actual content when revealed (`v-if="isRevealed"`)
- Only one displays at a time based on the `isRevealed` boolean state
- Vue's conditional rendering (`v-if`) removes the hidden span from DOM entirely

### Live Region for Screen Readers
```html
<div
  id="spoiler-announcement"
  aria-live="polite"
  aria-atomic="true"
  class="sr-only"
>
  {{ srAnnouncement }}
</div>
```

**What it does:**
- `aria-live="polite"`: Announces changes to screen readers without interrupting
- `aria-atomic="true"`: Reads the entire announcement, not just changes
- `.sr-only` class: Hides visually but keeps visible to assistive technology
- Updates dynamically when content is revealed

### Content Reference (Hidden Until Revealed)
```html
<div v-if="isRevealed" id="reveal-content" class="sr-only">
  {{ revealContent }}
</div>
```

**What it does:**
- **Only renders in DOM when `isRevealed` is true** - completely removed before clicking
- Screen readers cannot access content until button is clicked
- Provides a hidden reference for the `aria-controls` attribute on the button
- Maintains semantic connection between button and content
- Hidden visually with `.sr-only` but available to assistive technology when present

---

## ARIA Attributes (Accessibility)

### `aria-expanded`
```html
:aria-expanded="isRevealed"
```

**Why it's needed:**
- Tells screen readers if the button reveals hidden content (expanded) or hides it (collapsed)
- Value is `"true"` when revealed, `"false"` when hidden
- Screen readers announce this state when button is focused

**Screen Reader Output:**
- Hidden: Button is announced as "collapsed"
- Revealed: Button is announced as "expanded"

### `aria-label`
```html
:aria-label="ariaLabel"
```

**Why it's needed:**
- Provides the accessible name for the button
- Changes dynamically to reflect current state and content
- Overrides default button text announcements

**Content:**
```javascript
const ariaLabel = computed(() => {
  if (isRevealed.value) {
    return `${props.hideText}: ${props.revealContent}`  // e.g., "Click to hide: The cake is a lie"
  }
  return props.revealText  // e.g., "Click to reveal"
})
```

**Screen Reader Output:**
- Hidden: "Click to reveal"
- Revealed: "Click to hide: The cake is a lie"

### `aria-controls`
```html
:aria-controls="isRevealed ? 'reveal-content' : undefined"
```

**Why it's needed:**
- Creates a semantic relationship between the button and the content it controls
- **Only set when content exists in DOM** (`isRevealed ? 'reveal-content' : undefined`)
- When hidden: `aria-controls` is undefined (no broken references)
- When revealed: Points to the content element that now exists
- Screen readers understand the relationship and can navigate to revealed content
- Prevents referencing non-existent elements in the DOM

### `aria-atomic="true"` on Live Region
```html
<div aria-live="polite" aria-atomic="true" class="sr-only">
```

**Why it's needed:**
- `aria-atomic="true"`: Screen reader reads the entire message, not just what changed
- If omitted, might only announce "Spoiler revealed:" without the content
- Ensures users get the complete announcement

---

## Vue Reactive State Management

### Ref - Boolean State
```javascript
const isRevealed = ref(false)
```

**What it does:**
- Creates a reactive boolean that tracks if content is revealed
- Starts as `false` (hidden)
- When it changes, Vue automatically updates:
  - Button display
  - ARIA attributes
  - Live region announcement
  - CSS classes

**Initial Value:**
- `false` = content hidden, button shows "Click to reveal"

### Props - Configuration
```typescript
interface Props {
  revealContent: string      // The text to reveal
  revealText?: string        // "Click to reveal" text (default provided)
  hideText?: string          // "Click to hide" text (default provided)
}

const props = withDefaults(defineProps<Props>(), {
  revealText: 'Click to reveal',
  hideText: 'Click to hide',
})
```

**What it does:**
- `revealContent`: Required prop, the actual content to reveal
- `revealText`: Optional, customize the "click to reveal" button text
- `hideText`: Optional, used in aria-label when revealed
- `withDefaults`: Provides default values if props aren't passed

**Usage Example:**
```vue
<ClickToRevealButton 
  revealContent="The cake is a lie"
  revealText="Show me the truth"
  hideText="Hide the truth"
/>
```

---

## Computed Properties (Reactive Calculations)

### ariaLabel - Dynamic Accessibility Label
```javascript
const ariaLabel = computed(() => {
  if (isRevealed.value) {
    return `${props.hideText}: ${props.revealContent}`
  }
  return props.revealText
})
```

**What it does:**
- Computes the aria-label based on current state
- When hidden: Uses `revealText` (e.g., "Click to reveal")
- When revealed: Shows both hide text and actual content (e.g., "Click to hide: The cake is a lie")
- Automatically updates when `isRevealed` changes
- Screen reader users always know what the button will do

**Why Computed Property:**
- Reactive: Updates automatically when `isRevealed` changes
- More efficient than method (cached, only recalculates when dependencies change)
- Declarative: Clear intent that this value depends on state

### srAnnouncement - Screen Reader Announcement
```javascript
const srAnnouncement = computed(() => {
  if (isRevealed.value) {
    return `Content revealed: ${props.revealContent}`
  }
  return ''
})
```

**What it does:**
- When hidden: Returns empty string (nothing to announce)
- When revealed: Returns "Content revealed: [content]"
- Populates the live region
- Screen readers automatically announce when it changes

**Why This Works:**
- Empty string → No announcement when hiding
- Non-empty string → Announced when content reveals
- Live region watches for changes and announces them

---

## Event Handlers (Interactivity)

### Click Handler
```javascript
const toggleRevealed = () => {
  isRevealed.value = !isRevealed.value
}
```

**What it does:**
- Simple boolean toggle: `true` becomes `false`, `false` becomes `true`
- When clicked:
  1. `isRevealed` state changes
  2. Vue re-renders the component
  3. Button text switches (v-if changes what displays)
  4. ARIA attributes update
  5. Live region announcement triggers
  6. CSS classes update
  7. Visual styling changes

**HTML Event Binding:**
```html
@click="toggleRevealed"
```

**Keyboard Support:**
- Native `<button>` handles Enter and Space automatically
- No extra code needed; comes "for free" with semantic HTML
- Tab navigation also automatic

**Side Effects of Toggle:**
- When `isRevealed` becomes `true`:
  - `v-if="isRevealed"` renders the reveal-content div in DOM
  - `aria-controls` becomes `"reveal-content"` (now valid reference)
  - Content becomes available to screen readers
- When `isRevealed` becomes `false`:
  - `v-if="isRevealed"` removes reveal-content div from DOM completely
  - `aria-controls` becomes `undefined` (no broken reference)
  - Content is inaccessible to screen readers

---

## Vue Reactivity Flow

When user clicks the button:

```
1. User clicks button
   ↓
2. @click="toggleRevealed" fires
   ↓
3. toggleRevealed() function runs: isRevealed.value = !isRevealed.value
   ↓
4. isRevealed ref changes (reactive trigger)
   ↓
5. All dependent computations re-run:
   - ariaLabel computed property recalculates
   - srAnnouncement computed property recalculates
   ↓
6. Vue detects template changes and re-renders:
   - v-if="!isRevealed" evaluates (hiding/showing button text)
   - v-if="isRevealed" evaluates (hiding/showing content AND reveal-content div)
   - :aria-expanded="isRevealed" updates
   - :aria-label="ariaLabel" updates
   - :aria-controls updates (conditional: 'reveal-content' or undefined)
   - :class="{ revealed: isRevealed }" updates CSS classes
   - {{ srAnnouncement }} text updates
   ↓
7. DOM updates:
   - Button content switches from "Click to reveal" to actual content (or vice versa)
   - reveal-content div is added to DOM (when revealed) or removed (when hidden)
   - aria-expanded attribute changes value
   - aria-label attribute changes value
   - aria-controls attribute becomes valid reference or undefined
   - .revealed CSS class applied/removed
   - Live region text changes triggers screen reader announcement
   ↓
8. Screen readers detect aria-live region change
   ↓
9. Screen reader announces: "Content revealed: [content]"
```

---

## Why Content is Only Available When Revealed

The key to this component's accessibility is that **content doesn't exist in the DOM until clicked**:

### Before Clicking
```html
<!-- reveal-content div does NOT exist in DOM -->
<button aria-controls="undefined" aria-expanded="false">
  <span>Click to reveal</span>
</button>
<div aria-live="polite" class="sr-only"></div>
<!-- No reveal-content div -->
```

**Screen Reader Experience:**
- Focus button → Hears: "Click to reveal, button, collapsed"
- Content is completely unavailable
- Nothing to navigate to

### After Clicking
```html
<!-- reveal-content div NOW exists in DOM -->
<button aria-controls="reveal-content" aria-expanded="true" aria-label="Click to hide: The villain was the friend all along!">
  <span>The villain was the friend all along!</span>
</button>
<div aria-live="polite" class="sr-only">Content revealed: The villain was the friend all along!</div>
<!-- reveal-content div is now in DOM -->
<div id="reveal-content" class="sr-only">The villain was the friend all along!</div>
```

**Screen Reader Experience:**
- Live region announces: "Content revealed: The villain was the friend all along!"
- Button now has valid aria-controls reference
- Content is accessible and available for navigation
- User knows exactly what was revealed

### How This Works

1. **`v-if="isRevealed"` on reveal-content div**
   - Completely removes div from DOM when hidden
   - Adds div to DOM when revealed
   - Screen readers cannot access what doesn't exist

2. **`:aria-controls="isRevealed ? 'reveal-content' : undefined"`**
   - When hidden: `aria-controls` is undefined (no broken reference)
   - When revealed: `aria-controls="reveal-content"` points to existing div
   - Always semantically valid

3. **Live region announcement**
   - Provides the actual content to screen readers
   - User hears what was revealed
   - No need to navigate to discover content

---

### Dynamic Class Binding
```html
:class="{ revealed: isRevealed }"
```

**What it does:**
- Applies `.revealed` class when `isRevealed` is `true`
- Removes `.revealed` class when `isRevealed` is `false`
- Enables different visual states with CSS

### Button States
```css
.reveal-button {
  /* Default state - hidden */
  background-color: #f0f8ff;  /* Light blue */
  border-color: #0066cc;       /* Blue */
  color: #0066cc;
}

.reveal-button.revealed {
  /* Revealed state */
  background-color: #fff3cd;  /* Light gold */
  border-color: #ff9800;       /* Orange */
  color: #333;
}
```

**Visual Feedback:**
- Blue → Hidden state ("Click to reveal")
- Gold/Orange → Revealed state (shows content)
- Users always know which state they're in

### Hover & Focus States
```css
.reveal-button:hover:not(:disabled) {
  background-color: #e6f2ff;
  border-color: #0052a3;
}

.reveal-button:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
```

**Accessibility Benefits:**
- `:hover` provides mouse user feedback
- `:focus` provides keyboard user feedback
- 2px outline meets WCAG visibility standards
- `:not(:disabled)` prevents styling disabled buttons

### Screen Reader Only Class
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

**What it does:**
- Hides content visually (1x1 pixel, clipped off-screen)
- Still available to screen readers when the element exists in DOM
- Used for live region announcements and revealed content reference
- Not `display: none` or `visibility: hidden` so screen readers can access it
- Combined with `v-if`, controls when content is available to assistive technology

---

## Complete Flow Diagram

```
┌─────────────────────────────────────────┐
│   User Interaction (Click or Enter)     │
└────────────────┬────────────────────────┘
                 │
                 ▼
        ┌────────────────────┐
        │  toggleRevealed()  │
        │ Toggle isRevealed  │
        └────────┬───────────┘
                 │
                 ▼
    ┌────────────────────────────────┐
    │  Computed Properties Update:   │
    │  - ariaLabel recalculates      │
    │  - srAnnouncement recalculates │
    └────────┬───────────────────────┘
             │
             ▼
    ┌────────────────────────────────┐
    │    Vue Re-renders Template:    │
    │  - v-if directives evaluate    │
    │  - :aria-* attributes update   │
    │  - :class bindings update      │
    │  - {{ }} interpolations update │
    └────────┬───────────────────────┘
             │
             ▼
    ┌────────────────────────────────┐
    │      DOM Updates:              │
    │  - Button text switches        │
    │  - ARIA attributes change      │
    │  - CSS classes applied/removed │
    │  - Live region content changes │
    └────────┬───────────────────────┘
             │
             ▼
    ┌────────────────────────────────┐
    │  Browser & Assistive Tech:     │
    │  - Visual: Color + text change │
    │  - Screen Reader: Live region  │
    │    announcement triggered      │
    └────────────────────────────────┘
```

---

## Key Concepts Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| **Semantic HTML** | Proper keyboard support | `<button>` not `<div>` |
| **Reactive Ref** | Track state changes | `isRevealed` boolean |
| **Computed Property** | Derive values from state | `ariaLabel` updates automatically |
| **ARIA Attributes** | Communicate to screen readers | `aria-expanded`, `aria-label` |
| **Live Region** | Announce dynamic changes | `aria-live="polite"` |
| **Conditional Rendering** | Show/hide content | `v-if` directives |
| **Class Binding** | Style based on state | `:class="{ revealed: isRevealed }"` |
| **Event Binding** | Handle interactions | `@click="toggleRevealed"` |

---

## Why This Component is Accessible

✅ **Semantic HTML**: Native `<button>` = keyboard navigation for free  
✅ **Reactive ARIA**: `aria-expanded` and `aria-label` always in sync with state  
✅ **Live Announcements**: Screen readers notified of state changes  
✅ **Visual + Auditory**: Both sighted and screen reader users get feedback  
✅ **Keyboard Only**: Works with Tab + Enter/Space, no mouse needed  
✅ **Touch Friendly**: 44px minimum hit target size  
✅ **Focus Visible**: 2px outline clearly shows keyboard focus  
✅ **WCAG Compliant**: Meets AA standards for color contrast and semantics  
