# Contributing to SR_Testers

Thank you for your interest in contributing to SR_Testers! This is a library of fully accessible Vue components for screen reader testing.

## Code of Conduct

Please be respectful and inclusive in all interactions. Accessibility is about inclusion - let's maintain that spirit in our community.

## Getting Started

1. **Fork the repository**
   ```bash
   git clone https://github.com/danielwestfall-pixel/SR_Testers.git
   cd SR_Testers
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Create a new branch**
   ```bash
   git checkout -b feature/your-component-name
   ```

## Adding a New Component

See [ADDING_COMPONENTS.md](./ADDING_COMPONENTS.md) for detailed step-by-step instructions.

**Quick checklist:**
- [ ] Component folder created under `src/components/`
- [ ] Component file implements proper semantics
- [ ] `index.ts` export created
- [ ] Export added to `src/components/index.ts`
- [ ] Demo component created
- [ ] Demo integrated into `DemoPage.vue`
- [ ] Component tested with screen reader
- [ ] Component tested with keyboard only
- [ ] Focus management verified
- [ ] Color contrast verified (WCAG AA minimum)
- [ ] Mobile/touch testing completed

## Accessibility Requirements

Every component in this library MUST meet these standards:

### Semantic HTML
- ✅ Use native HTML elements (`<button>`, `<input>`, etc.)
- ✅ Avoid using `<div>` and `<span>` for interactive elements
- ✅ Proper heading hierarchy

### ARIA Attributes
- ✅ `aria-label` or `aria-labelledby` for controls
- ✅ `aria-expanded` for toggles/disclosures
- ✅ `aria-checked` for checkboxes/switches
- ✅ `aria-live` regions for dynamic content
- ✅ Proper `role` attributes when needed

### Keyboard Navigation
- ✅ Tab navigation support
- ✅ Enter/Space to activate buttons
- ✅ Arrow keys for list/menu navigation
- ✅ Escape to close modals/popovers
- ✅ Proper focus management
- ✅ Visible focus indicators (2px outline minimum)

### Screen Reader Support
- ✅ All information available to screen readers
- ✅ Proper announcements for state changes
- ✅ Live regions for dynamic updates
- ✅ Semantic relationships between elements

### Visual Accessibility
- ✅ WCAG AA color contrast (4.5:1 for text)
- ✅ 44x44px minimum touch targets
- ✅ Clear visual focus indicators
- ✅ No color-only information conveyance

### Testing
- ✅ Tested with NVDA (Windows)
- ✅ Tested with VoiceOver (macOS/iOS)
- ✅ Keyboard-only navigation
- ✅ High contrast mode
- ✅ 200% zoom level

See [TESTING_GUIDE.md](./TESTING_GUIDE.md) for detailed testing procedures.

## Testing with Screen Readers

### Before Submitting PR

Test your component with:

**Windows:**
- NVDA (free): https://www.nvaccess.org/
- Windows Narrator (built-in): Win+Ctrl+N

**macOS/iOS:**
- VoiceOver (built-in): Cmd+F5

**Android:**
- TalkBack (built-in): Settings → Accessibility

Follow the testing checklist in [TESTING_GUIDE.md](./TESTING_GUIDE.md).

## Code Style

### Vue Components
```vue
<template>
  <!-- Semantic HTML first -->
  <button aria-label="..." aria-expanded="isExpanded">
    Click me
  </button>
</template>

<script setup lang="ts">
// Use TypeScript for type safety
import { ref, computed } from 'vue'

interface Props {
  label: string
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
})

const isExpanded = ref(false)

// Use computed properties for reactive values
const ariaLabel = computed(() => {
  return props.label
})
</script>

<style scoped>
/* Scoped styles only */
.component {
  /* Styles */
}
</style>
```

### TypeScript
- Use explicit types (no `any`)
- Define interfaces for props and emits
- Use `const` and `let` appropriately

### CSS
- Use CSS variables for colors/spacing
- Support high contrast mode: `@media (prefers-contrast: more)`
- Support dark mode: `@media (prefers-color-scheme: dark)`
- Use relative units (`rem`, `em`) where possible
- Scoped styles only (`:scoped`)

## Commit Messages

Use clear, descriptive commit messages:

```
feat(ClickToRevealButton): Add component to library
docs(ADDING_COMPONENTS): Update guide with examples
fix(ClickToRevealButton): Ensure aria-controls only set when content exists
test(ClickToRevealButton): Add screen reader testing notes
```

**Format**: `type(component): description`

**Types**:
- `feat`: New feature or component
- `fix`: Bug fix
- `docs`: Documentation changes
- `test`: Testing or test-related changes
- `refactor`: Code refactoring
- `style`: CSS/styling changes
- `chore`: Dependencies, tooling, etc.

## Pull Request Process

1. **Update your branch**
   ```bash
   git pull origin main
   ```

2. **Run tests**
   ```bash
   npm run build
   ```

3. **Test in demo**
   ```bash
   npm run dev
   # Visit http://localhost:5173
   ```

4. **Push changes**
   ```bash
   git push origin feature/your-component-name
   ```

5. **Open Pull Request**
   - Title: Clear description of changes
   - Description: Include testing notes, accessibility checklist
   - Reference any related issues

### PR Checklist
- [ ] Code follows style guidelines
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No console errors/warnings
- [ ] Tested with at least one screen reader
- [ ] Keyboard navigation tested
- [ ] Focus management verified
- [ ] Color contrast verified
- [ ] Mobile/touch tested

## Development Commands

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Documentation

When adding components, update:

1. **[ADDING_COMPONENTS.md](./ADDING_COMPONENTS.md)** - Add example if applicable
2. **[LIBRARY_STRUCTURE.md](./LIBRARY_STRUCTURE.md)** - Add to file structure if needed
3. **Component README** - Consider adding README in component folder
4. **Demo file** - Create comprehensive demo with examples

## Reporting Issues

Found an accessibility issue? Please create an issue with:

- **Title**: Clear description (e.g., "ClickToRevealButton not keyboard accessible")
- **Component**: Which component is affected
- **Environment**: Browser, screen reader, OS
- **Steps to reproduce**: Clear steps to see the issue
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Screenshots/Video**: If applicable

## Questions?

- Check [HOW_IT_WORKS.md](./HOW_IT_WORKS.md) to understand component internals
- See [ADDING_COMPONENTS.md](./ADDING_COMPONENTS.md) for implementation guide
- Review [TESTING_GUIDE.md](./TESTING_GUIDE.md) for testing procedures
- Open an issue for questions

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Recognition

Contributors will be acknowledged in:
- Pull request comments
- CHANGELOG
- Contributors list (if one exists)

---

Thank you for helping make accessible components! 🎉
