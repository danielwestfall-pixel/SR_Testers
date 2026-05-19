# Adding New Components to the Library

This is a structured component library for accessibility-tested Vue components. Follow these steps to add new components.

## Project Structure

```
src/
├── components/           # All reusable components
│   ├── index.ts         # Exports all components
│   ├── ClickToRevealButton/
│   │   ├── ClickToRevealButton.vue
│   │   └── index.ts
│   └── YourComponent/   # New component (follows same pattern)
│       ├── YourComponent.vue
│       └── index.ts
├── demo/                # Demo pages for each component
│   ├── DemoPage.vue    # Main demo page (entry point)
│   ├── ClickToRevealButtonDemo.vue
│   └── YourComponentDemo.vue
├── App.vue
└── main.ts
```

## Step-by-Step: Adding a New Component

### 1. Create Component Folder
```bash
mkdir src/components/YourComponentName
```

### 2. Create the Component File
Create `src/components/YourComponentName/YourComponentName.vue`:

```vue
<template>
  <div class="your-component">
    <!-- Your component template -->
  </div>
</template>

<script setup lang="ts">
// Your component logic
</script>

<style scoped>
/* Your component styles */
</style>
```

### 3. Create Component Export File
Create `src/components/YourComponentName/index.ts`:

```typescript
export { default as YourComponentName } from './YourComponentName.vue'
```

### 4. Add to Main Components Export
Update `src/components/index.ts`:

```typescript
// Export all testing components
export * from './ClickToRevealButton'
export * from './YourComponentName'  // Add this line
```

### 5. Create Demo Component
Create `src/demo/YourComponentNameDemo.vue`:

```vue
<template>
  <div class="demo-container">
    <h1>Accessible Your Component Name</h1>
    
    <section class="demo-section">
      <h2>Basic Example</h2>
      <YourComponentName />
    </section>

    <section class="demo-section">
      <h2>Features</h2>
      <ul>
        <li>Feature 1</li>
        <li>Feature 2</li>
      </ul>
    </section>
  </div>
</template>

<script setup lang="ts">
import { YourComponentName } from '../components'
</script>

<style scoped>
/* Demo styles */
</style>
```

### 6. Add to Main Demo Page
Update `src/demo/DemoPage.vue` to include your component:

```vue
<template>
  <div class="demo-page">
    <!-- ... existing header and nav ... -->
    
    <nav class="demo-nav">
      <ul>
        <li><a href="#click-to-reveal">Click to Reveal</a></li>
        <li><a href="#your-component">Your Component</a></li>  <!-- Add this -->
      </ul>
    </nav>

    <main class="demo-content">
      <!-- ... existing sections ... -->
      
      <section id="your-component" class="component-demo">
        <YourComponentNameDemo />
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import ClickToRevealButtonDemo from './ClickToRevealButtonDemo.vue'
import YourComponentNameDemo from './YourComponentNameDemo.vue'  <!-- Add this -->
</script>
```

## Using Components Elsewhere

Once added to the library, import from the main components export:

```typescript
import { ClickToRevealButton, YourComponentName } from './components'
```

Or import a specific component:

```typescript
import { YourComponentName } from './components/YourComponentName'
```

## Testing for Accessibility

Each new component should include:
- ✅ Semantic HTML (proper element types)
- ✅ ARIA attributes (labels, roles, expanded states)
- ✅ Keyboard navigation support
- ✅ Screen reader announcements
- ✅ Focus management
- ✅ Color contrast compliance
- ✅ Touch target sizes (44x44px minimum)

See [TESTING_GUIDE.md](../TESTING_GUIDE.md) for detailed testing procedures.

## Documentation

Each component should have:
1. **HOW_IT_WORKS.md** - Technical breakdown (consider adding to component folder)
2. **Component comments** - Inline JSDoc for props and emits
3. **Demo component** - Multiple usage examples
4. **README** - Usage guide and API (consider component-specific README)

## Example: Adding "Toggle Switch" Component

### File: `src/components/ToggleSwitch/ToggleSwitch.vue`
```vue
<template>
  <button
    role="switch"
    :aria-checked="isActive"
    :aria-label="ariaLabel"
    class="toggle-switch"
    :class="{ active: isActive }"
    @click="toggle"
  >
    <span class="toggle-slider"></span>
  </button>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Props {
  modelValue?: boolean
  label?: string
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: false,
  label: 'Toggle switch',
})

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()

const isActive = ref(props.modelValue)

const ariaLabel = computed(() => props.label)

const toggle = () => {
  isActive.value = !isActive.value
  emit('update:modelValue', isActive.value)
}
</script>

<style scoped>
/* Styles */
</style>
```

### File: `src/components/ToggleSwitch/index.ts`
```typescript
export { default as ToggleSwitch } from './ToggleSwitch.vue'
```

### File: `src/demo/ToggleSwitchDemo.vue`
```vue
<template>
  <div class="demo-container">
    <h1>Accessible Toggle Switch</h1>
    <section class="demo-section">
      <h2>Basic Example</h2>
      <ToggleSwitch v-model="isEnabled" label="Enable feature" />
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { ToggleSwitch } from '../components'

const isEnabled = ref(false)
</script>

<style scoped>
/* Styles */
</style>
```

## File Naming Conventions

- **Component files**: PascalCase (e.g., `ClickToRevealButton.vue`)
- **Component folders**: PascalCase (e.g., `ClickToRevealButton/`)
- **Demo files**: ComponentNameDemo.vue
- **Index files**: Always `index.ts` or `index.vue`

## Building the Library

```bash
# Build for production
npm run build

# Output goes to dist/
```

## Quick Checklist for New Components

- [ ] Created component folder under `src/components/`
- [ ] Created component `.vue` file
- [ ] Created component `index.ts` export
- [ ] Added export to `src/components/index.ts`
- [ ] Created demo component file
- [ ] Added demo import to `DemoPage.vue`
- [ ] Added navigation link to `DemoPage.vue`
- [ ] Tested keyboard navigation
- [ ] Tested with screen reader
- [ ] Verified focus management
- [ ] Checked color contrast
- [ ] Tested on mobile/touch
- [ ] Added documentation/comments

---

For detailed accessibility testing, see [TESTING_GUIDE.md](../TESTING_GUIDE.md).
