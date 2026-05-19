<template>
  <div class="demo-container">
    <h1>Accessible Click to Reveal Button Component</h1>

    <section class="demo-section">
      <h2>Basic Example</h2>
      <ClickToRevealButton
        revealContent="The villain was the friend all along!"
      />
    </section>

    <section class="demo-section">
      <h2>Accessibility Features</h2>
      <ul>
        <li>
          <strong>Semantic Button:</strong> Uses native HTML button element for
          keyboard navigation
        </li>
        <li>
          <strong>Keyboard Support:</strong> Navigate with Tab, activate with
          Enter or Space
        </li>
        <li>
          <strong>ARIA Labels:</strong> aria-label dynamically updates to
          describe state
        </li>
        <li>
          <strong>ARIA Expanded:</strong> aria-expanded indicates open/closed
          state
        </li>
        <li>
          <strong>Live Region:</strong> aria-live announces content reveal to
          screen readers
        </li>
        <li>
          <strong>Focus Visible:</strong> Clear focus indicator for keyboard
          users
        </li>
        <li>
          <strong>Touch Target:</strong> Minimum 44x44px for mobile
          accessibility
        </li>
        <li><strong>Color Contrast:</strong> Meets WCAG AA standards</li>
      </ul>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ClickToRevealButton } from '../components'
</script>

<style scoped>
.demo-container {
  padding: 2rem;
  max-width: 800px;
  margin: 0 auto;
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu,
    Cantarell, sans-serif;
}

h1 {
  color: #333;
  margin-bottom: 2rem;
  border-bottom: 3px solid #0066cc;
  padding-bottom: 1rem;
}

h2 {
  color: #555;
  font-size: 1.25rem;
  margin-top: 2rem;
  margin-bottom: 1rem;
}

.demo-section {
  margin-bottom: 3rem;
  padding: 1.5rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0066cc;
  border-radius: 4px;
}

.demo-section ul {
  list-style-position: inside;
  line-height: 1.8;
  color: #666;
}

.demo-section li {
  margin-bottom: 0.5rem;
}

.demo-section strong {
  color: #333;
}
</style>
