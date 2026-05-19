# Component Library Structure

Your project is now organized as a scalable component library. Here's how it works:

## Directory Structure

```
Vue Components for SR Testing/
├── src/
│   ├── components/              # Reusable components
│   │   ├── index.ts            # Exports all components
│   │   └── ClickToRevealButton/
│   │       ├── ClickToRevealButton.vue
│   │       └── index.ts
│   ├── demo/                    # Demo pages for testing
│   │   ├── DemoPage.vue        # Main entry point (shows all components)
│   │   └── ClickToRevealButtonDemo.vue
│   ├── App.vue                  # App root
│   ├── main.ts                  # Entry point
│   └── index.ts                 # Library export
├── index.html
├── vite.config.ts
├── package.json
├── HOW_IT_WORKS.md              # How components work internally
├── ADDING_COMPONENTS.md         # Guide to add new components
├── TESTING_GUIDE.md             # Screen reader testing guide
├── SETUP.md                     # Setup instructions
└── README.md                    # Component documentation
```

## How It Works

### Components

Each component lives in its own folder under `src/components/`:

```
ClickToRevealButton/
├── ClickToRevealButton.vue      # Component implementation
└── index.ts                     # Named export
```

The `index.ts` file exports the component:
```typescript
export { default as ClickToRevealButton } from './ClickToRevealButton.vue'
```

### Main Components Export

`src/components/index.ts` collects all exports:
```typescript
export * from './ClickToRevealButton'
export * from './NextComponent'  // Add future components here
```

This allows importing like:
```typescript
import { ClickToRevealButton, NextComponent } from './components'
```

### Demo System

- `src/demo/DemoPage.vue` - Main demo page that shows all components
- `src/demo/ClickToRevealButtonDemo.vue` - Individual component demo
- Each demo imports its component from the library

The demo is accessible via:
```
http://localhost:5173/
```

Navigation links jump to each component's demo section.

## Adding a New Component

See [ADDING_COMPONENTS.md](./ADDING_COMPONENTS.md) for detailed instructions.

**Quick summary:**
1. Create `src/components/YourComponent/YourComponent.vue`
2. Create `src/components/YourComponent/index.ts` with export
3. Add export to `src/components/index.ts`
4. Create demo at `src/demo/YourComponentDemo.vue`
5. Add demo import and section to `src/demo/DemoPage.vue`

## Importing Components

### From Library
```typescript
import { ClickToRevealButton } from './components'
```

### From Specific Component
```typescript
import { ClickToRevealButton } from './components/ClickToRevealButton'
```

### In Demos
```typescript
import { ClickToRevealButton } from '../components'
```

## Running the Demo

```bash
npm run dev
```

Opens `http://localhost:5173` with:
- Navigation to each component
- Individual component demos
- Live editing with hot reload

## Building for Production

```bash
npm run build
```

Creates optimized bundle in `dist/`

## Scalability

This structure supports:
- ✅ Multiple components (add one per folder)
- ✅ Shared utilities (create `src/utils/`)
- ✅ Shared styles (create `src/styles/`)
- ✅ Type definitions (add `.d.ts` files)
- ✅ Component composition (components can use other components)
- ✅ Demo variations (multiple demos per component)

## File Organization Best Practices

### By Feature (Current)
```
components/
├── ClickToRevealButton/
├── ToggleSwitch/
└── Modal/
```

This works well for a small-to-medium library.

### As Library Grows
Consider adding utilities:
```
components/
├── ClickToRevealButton/
├── ToggleSwitch/
└── Modal/
utils/
├── accessibility.ts
├── keyboard.ts
└── focus.ts
styles/
├── common.css
├── colors.css
└── spacing.css
```

## Next Steps

1. ✅ Project is structured and ready
2. ✅ ClickToRevealButton component included
3. 📝 Add more components following [ADDING_COMPONENTS.md](./ADDING_COMPONENTS.md)
4. 📋 Test each component with [TESTING_GUIDE.md](./TESTING_GUIDE.md)
5. 🚀 Build and deploy when ready

---

Each new component you add will automatically appear in:
- The demo navigation
- The library exports
- The build output

This structure makes it easy to manage and grow your accessibility testing component library! 🎯
