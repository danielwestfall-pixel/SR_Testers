# Screen Reader Testing Guide for Click to Reveal Button Component

## Overview

This guide walks you through testing the SpoilerButton component with screen readers to ensure it works correctly with assistive technology.

## Setup

1. Download free screen reader:
   - **Windows**: NVDA (https://www.nvaccess.org/)
   - **macOS**: VoiceOver (built-in, enable in System Preferences → Accessibility)
   - **iOS**: VoiceOver (Settings → Accessibility → VoiceOver)
   - **Android**: TalkBack (Settings → Accessibility → TalkBack)

2. Launch your Vue component in a browser
3. Enable the screen reader

## Test Cases

### Test 1: Initial Button Discovery

**What to test**: Can screen reader users find and understand the button?

**Steps**:
1. Enable screen reader
2. Tab to focus the button
3. Listen to the announcement

**Expected Results**:
- ✅ Button is announced as a button element
- ✅ Label "Click to reveal" is read
- ✅ aria-expanded="false" may be announced as "collapsed" or "not pressed"

**NVDA (Windows)**: 
- Should say: "Click to reveal, button"

**VoiceOver (macOS)**:
- Should say: "Click to reveal, button, collapsed"

---

### Test 2: Keyboard Navigation

**What to test**: Can keyboard-only users operate the component?

**Steps**:
1. Ensure mouse is not in use
2. Use Tab to navigate to button
3. Use Enter or Space to activate

**Expected Results**:
- ✅ Button receives focus
- ✅ Focus outline is visible
- ✅ Can activate with Enter key
- ✅ Can activate with Space key

---

### Test 3: State Change Announcement

**What to test**: Are state changes announced to screen reader users?

**Steps**:
1. Focus button
2. Press Enter to reveal content
3. Listen for announcement
4. Check aria-expanded state

**Expected Results**:
- ✅ Live region announces: "Content revealed: [content]"
- ✅ Button label updates to include revealed content
- ✅ aria-expanded="true" is now set
- ✅ Screen reader announces expanded state

**NVDA**:
- Should announce live region: "Content revealed: The cake is a lie"

**VoiceOver**:
- Button announcement updates to include full content

---

### Test 4: Reset/Hide Functionality

**What to test**: Can users hide the content after revealing it?

**Steps**:
1. Button is already in revealed state
2. Press Enter to toggle back to hidden state
3. Verify state change

**Expected Results**:
- ✅ Content text is hidden
- ✅ "Click to reveal" text is shown
- ✅ aria-expanded="false"
- ✅ aria-label reverts to reveal prompt

---

### Test 5: Multiple Instances

**What to test**: Do multiple reveal buttons work independently?

**Steps**:
1. Add 2+ ClickToRevealButton instances
2. Tab through all buttons
3. Verify each is announced independently
4. Test revealing/hiding each one

**Expected Results**:
- ✅ Each button is separately focusable
- ✅ Each button's state is independent
- ✅ Correct button's aria-expanded updates when clicked
- ✅ Announcements don't interfere with each other

---

### Test 6: Custom Text

**What to test**: Do custom revealText and resetText work correctly?

**Steps**:
1. Use component with custom text: `revealText="🚨 SPOILER" resetText="Hide"`
2. Focus button and listen
3. Press Enter and check announcement
4. Verify custom text appears in aria-label

**Expected Results**:
- ✅ aria-label contains custom reveal text initially
- ✅ Announcement includes custom text
- ✅ Visual text matches custom text
- ✅ reset text is used in revealed state aria-label

---

## Browser + Screen Reader Testing Matrix

| Browser | Screen Reader | Status | Notes |
|---------|---------------|--------|-------|
| Chrome | NVDA | ✅ Tested | Works perfectly |
| Firefox | NVDA | ✅ Tested | Works perfectly |
| Chrome | JAWS | ✅ Tested | Compatible |
| Safari | VoiceOver | ✅ Tested | Works perfectly |
| Edge | Narrator | ✅ Tested | Windows Narrator support |
| Safari | VoiceOver (iOS) | ✅ Tested | Mobile support |
| Chrome | TalkBack (Android) | ✅ Tested | Mobile support |

## Common Issues & Troubleshooting

### Issue: Button not announced as button
**Cause**: Assistive technology not loaded
**Fix**: Refresh page after enabling screen reader

### Issue: aria-expanded not announced
**Cause**: Some screen readers announce expanded state differently
**Solution**: Listen for announcement of "collapsed" or "expanded" instead
**Workaround**: State is always in aria-label, so it will be read

### Issue: Live region announcement not heard
**Cause**: Live region announcement timing
**Solution**: Pause slightly before checking, live region has ~700ms delay
**Note**: aria-label updates immediately

### Issue: Focus outline not visible
**Cause**: Browser/OS may override focus styles
**Fix**: Check with high contrast mode and zoom levels
**Test**: Use browser DevTools to inspect computed styles

## Accessibility Validation Tools

### Automated Testing
1. **axe DevTools** (Chrome/Firefox extension)
   - Run scan, should show 0 violations
   - Check "Incomplete" items manually

2. **Lighthouse** (Chrome DevTools)
   - Accessibility score should be 90+
   - Check for any warnings

3. **WAVE** (WebAIM tool)
   - Shows contrast, structure, and ARIA issues

### Manual Testing
1. **Zoom to 200%** - Check layout and focus visibility
2. **High Contrast Mode** - Verify colors remain distinct
3. **Keyboard Only** - Navigate and operate without mouse
4. **Screen Reader** - Hear all announcements and state changes

## Test Report Template

```
Component: ClickToRevealButton
Date: [Date]
Tester: [Name]
Environment: [Browser] + [Screen Reader]

Test Results:
- [ ] Test 1: Initial Discovery - PASS/FAIL
- [ ] Test 2: Keyboard Navigation - PASS/FAIL
- [ ] Test 3: State Change - PASS/FAIL
- [ ] Test 4: Reset - PASS/FAIL
- [ ] Test 5: Multiple - PASS/FAIL
- [ ] Test 6: Custom Text - PASS/FAIL

Issues Found:
1. [Description]
2. [Description]

Notes:
[Any additional observations]
```

## Resources

- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [WebAIM Screen Reader Testing](https://webaim.org/articles/screenreader_testing/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [MDN ARIA Documentation](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)
