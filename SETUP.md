# Setup & Run Guide

## Quick Start

### 1. Install Dependencies
```bash
npm install
```

### 2. Run Development Server
```bash
npm run dev
```

The browser will automatically open to `http://localhost:5173` where you can see the component demo.

### 3. Test with Screen Reader

#### On Windows:
1. **NVDA (Free)**
   - Download from https://www.nvaccess.org/
   - Install and launch
   - Focus the buttons with Tab
   - Listen to announcements
   
2. **Windows Narrator (Built-in)**
   - Press `Windows Key + Ctrl + N` to toggle
   - Tab to navigate buttons

#### On macOS:
1. **VoiceOver (Built-in)**
   - Press `Cmd + F5` to toggle
   - Press `VO + U` to open the rotor for navigation

#### On iOS/Android:
- **VoiceOver** (iOS): Settings → Accessibility → VoiceOver → Toggle On
- **TalkBack** (Android): Settings → Accessibility → TalkBack → Toggle On

### 4. Test Keyboard Navigation

With the dev server running:
1. Use **Tab** to move focus to buttons
2. Use **Shift+Tab** to move focus backward
3. Press **Enter** or **Space** to activate the button
4. The visual display and screen reader announcements should update

## What to Test

✅ **Keyboard Navigation**
- Tab through all buttons
- Shift+Tab to go back
- Enter/Space activates button

✅ **Screen Reader**
- Focus each button
- Hear "Click to reveal" initially
- Press Enter
- Hear live region announcement: "Content revealed: [content]"
- Button state updates to "revealed"
- Can press Enter again to hide

✅ **Visual States**
- Blue button shows "Click to reveal"
- Orange/yellow button shows spoiler text
- Hover effects work
- Focus outline is visible

✅ **Responsive**
- Resize browser window
- Test on mobile/tablet
- Touch interactions work

## Build for Production

```bash
npm run build
```

Output will be in the `dist/` folder.

## Project Structure

```
.
├── src/
│   ├── components/
│   │   ├── ClickToRevealButton.vue       # Main component
│   │   └── ClickToRevealButtonDemo.vue   # Demo page
│   ├── App.vue                           # App root
│   └── main.ts                           # Entry point
├── index.html                            # HTML template
├── vite.config.ts                        # Vite configuration
├── tsconfig.json                         # TypeScript config
├── package.json                          # Dependencies
└── README.md                             # Component docs
```

## Troubleshooting

**Port already in use**
- Vite will try 5174, 5175, etc. automatically
- Or change port in `vite.config.ts`

**Module not found errors**
- Delete `node_modules` and `package-lock.json`
- Run `npm install` again

**Screen reader not announcing**
- Refresh the page (F5)
- Ensure screen reader is running
- Check browser accessibility settings

**Build fails**
- Ensure Node.js 16+ is installed
- Run `npm install` to update dependencies
- Check for TypeScript errors: `npm run build`

## Testing Accessibility

See [TESTING_GUIDE.md](TESTING_GUIDE.md) for comprehensive testing procedures with different screen readers.
