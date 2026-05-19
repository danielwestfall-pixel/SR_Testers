# Accessible Click to Reveal Button Component

A fully accessible Vue 3 component that reveals/hides content with comprehensive screen reader support and keyboard navigation.

**Repository**: [github.com/danielwestfall-pixel/SR_Testers](https://github.com/danielwestfall-pixel/SR_Testers)

## Features

### ✅ Accessibility

- **Semantic HTML**: Uses native `<button>` element for proper keyboard navigation
- **ARIA Attributes**:
  - `aria-expanded`: Indicates whether the spoiler is revealed
  - `aria-label`: Dynamically updated to describe current state
  - `aria-controls`: Links button to controlled content
  - `aria-live="polite"`: Announces spoiler reveal to screen readers
  - `aria-atomic="true"`: Entire announcement is read, not just changes
- **Keyboard Support**: 
  - Tab to focus
  - Enter or Space to toggle
- **Focus Management**: Clear focus indicator with 2px outline
- **Touch Targets**: Minimum 44x44px for mobile accessibility
- **Color Contrast**: Meets WCAG AA standards (4.5:1 minimum)
- **Screen Reader Announcements**: Live region provides context-aware announcements

### 🎨 Visual States

- **Default**: Blue button with "Click to reveal" text
- **Revealed**: Golden/warning color with spoiler text displayed
- **Hover**: Color changes for feedback
- **Focus**: Clear outline for keyboard users

## Usage

### Basic Example

```vue
<template>
  <ClickToRevealButton revealContent="The cake is a lie" />
</template>

<script setup>
import ClickToRevealButton from './ClickToRevealButton.vue'
</script>
```

### With Custom Text

```vue
<ClickToRevealButton
  revealContent="Luke is Vader's son"
  revealText="🚨 Major content!"
  hideText="Hide content"
/>
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `revealContent` | `string` | **Required** | The content text to reveal |
| `revealText` | `string` | `"Click to reveal"` | Button text before reveal |
| `hideText` | `string` | `"Click to hide"` | Used in aria-label when revealed |

## Screen Reader Behavior

### NVDA (Windows)
When the button is focused:
- Initially: "Click to reveal, button"
- After clicking: "Content revealed: [content], button, expanded"

### JAWS (Windows)
When the button is focused:
- Initially: "Click to reveal button"
- After clicking: Live region announces "Content revealed: [content]"

### VoiceOver (macOS/iOS)
- Full state and content information in button label
- Live region provides announcement on reveal

## Styling Customization

The component uses scoped CSS with class names you can override:

- `.reveal-container`: Wrapper element
- `.reveal-button`: Main button
- `.reveal-button.revealed`: Button in revealed state
- `.reveal-text`: The "Click to reveal" text
- `.hidden-text`: The actual hidden content
- `.sr-only`: Screen reader only class (hidden visually)

Example custom styling:

```vue
<style>
:deep(.reveal-button) {
  background-color: #e91e63;
  border-color: #c2185b;
}

:deep(.reveal-button:hover) {
  background-color: #f50057;
}

:deep(.reveal-button.revealed) {
  background-color: #4caf50;
}
</style>
```

## Testing with Screen Readers

### Testing Checklist

- [ ] Navigate to button with Tab key
- [ ] Button is announced with its label
- [ ] Press Enter/Space to toggle
- [ ] aria-expanded updates (announced or available)
- [ ] Content is revealed
- [ ] Focus outline remains visible
- [ ] Screen reader announces content reveal in live region
- [ ] Clicking again hides content
- [ ] All interactive elements are keyboard accessible
- [ ] Color contrast is sufficient (use Chrome DevTools)

### Browsers & Assistive Technology

- ✅ Chrome + NVDA (Windows)
- ✅ Firefox + NVDA (Windows)
- ✅ Safari + VoiceOver (macOS)
- ✅ Safari + VoiceOver (iOS)
- ✅ Chrome + TalkBack (Android)

## Accessibility Standards

This component conforms to:
- **WCAG 2.1 Level AA**: General accessibility guidelines
- **WAI-ARIA 1.2**: Accessible Rich Internet Applications specification
- **ATAG 2.0**: Authoring Tool Accessibility Guidelines

## Vue Version

- Vue 3.x (with TypeScript support)

## License

MIT
