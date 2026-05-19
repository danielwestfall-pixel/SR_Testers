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

    <section class="demo-section">
      <button
        class="section-toggle"
        :aria-expanded="showHowToUse"
        @click="showHowToUse = !showHowToUse"
      >
        <span class="toggle-icon">{{ showHowToUse ? '▼' : '▶' }}</span>
        How to Use
      </button>
      
      <div v-if="showHowToUse" class="collapsible-content">
        <h3>Import</h3>
      <div class="code-block">
        <pre><code>import {{ ClickToRevealButton }} from './components'</code></pre>
      </div>

      <h3>Basic Usage</h3>
      <div class="code-block">
        <pre><code>&lt;template&gt;
  &lt;ClickToRevealButton
    revealContent="The villain was the friend all along!"
  /&gt;
&lt;/template&gt;

&lt;script setup&gt;
import {{ ClickToRevealButton }} from './components'
&lt;/script&gt;</code></pre>
      </div>

      <h3>With Custom Text</h3>
      <div class="code-block">
        <pre><code>&lt;ClickToRevealButton
  revealContent="The cake is a lie"
  revealText="Show me the truth"
  hideText="Hide the truth"
/&gt;</code></pre>
      </div>

      <h3>Props</h3>
      <table class="props-table">
        <thead>
          <tr>
            <th>Prop</th>
            <th>Type</th>
            <th>Default</th>
            <th>Description</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><code>revealContent</code></td>
            <td>string</td>
            <td>Required</td>
            <td>The content text to reveal when button is clicked</td>
          </tr>
          <tr>
            <td><code>revealText</code></td>
            <td>string</td>
            <td>"Click to reveal"</td>
            <td>Button label text before content is revealed</td>
          </tr>
          <tr>
            <td><code>hideText</code></td>
            <td>string</td>
            <td>"Click to hide"</td>
            <td>Used in aria-label when content is revealed</td>
          </tr>
        </tbody>
      </table>

      <h3>How It Works</h3>
      <ul>
        <li><strong>Initial State:</strong> Button shows "Click to reveal" in blue</li>
        <li><strong>After Click:</strong> Button shows revealed content in gold/orange</li>
        <li><strong>Screen Reader:</strong> Announces "Content revealed: [content]" in live region</li>
        <li><strong>Keyboard:</strong> Tab to focus, Enter or Space to toggle</li>
        <li><strong>State:</strong> Content is only in DOM when revealed (not accessible until clicked)</li>
      </ul>

      <h3>State Management</h3>
      <div class="code-block">
        <pre><code>// Component manages state internally
const isRevealed = ref(false)

// Toggle on click
const toggleRevealed = () => {
  isRevealed.value = !isRevealed.value
}</code></pre>
      </div>

      <h3>Accessibility Implementation</h3>
      <div class="code-block">
        <pre><code>&lt;button
  :aria-expanded="isRevealed"
  :aria-label="ariaLabel"
  :aria-controls="isRevealed ? 'reveal-content' : undefined"
  @click="toggleRevealed"
&gt;
  &lt;span v-if="!isRevealed"&gt;Click to reveal&lt;/span&gt;
  &lt;span v-if="isRevealed"&gt;{{ revealContent }}&lt;/span&gt;
&lt;/button&gt;

&lt;!-- Screen reader announcement --&gt;
&lt;div aria-live="polite" aria-atomic="true" class="sr-only"&gt;
  {{ srAnnouncement }}
&lt;/div&gt;

&lt;!-- Content reference (only in DOM when revealed) --&gt;
&lt;div v-if="isRevealed" id="reveal-content" class="sr-only"&gt;
  {{ revealContent }}
&lt;/div&gt;</code></pre>
      </div>
      </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { ClickToRevealButton } from '../components'

const showHowToUse = ref(false)
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

h3 {
  color: #666;
  font-size: 1.05rem;
  margin-top: 1.5rem;
  margin-bottom: 0.75rem;
}

.code-block {
  background-color: #2d2d2d;
  color: #f8f8f2;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
  margin: 1rem 0;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  line-height: 1.5;
}

.code-block pre {
  margin: 0;
  font-family: inherit;
}

.code-block code {
  color: #f8f8f2;
}

.props-table {
  width: 100%;
  border-collapse: collapse;
  margin: 1rem 0;
  background-color: white;
}

.props-table th,
.props-table td {
  padding: 0.75rem;
  text-align: left;
  border-bottom: 1px solid #ddd;
  color: #333;
}

.props-table th {
  background-color: #f0f8ff;
  color: #0066cc;
  font-weight: 600;
}

.props-table tr:hover {
  background-color: #f9f9f9;
}

.props-table code {
  background-color: #f0f8ff;
  padding: 0.2rem 0.4rem;
  border-radius: 2px;
  color: #0066cc;
  font-family: 'Courier New', monospace;
  font-size: 0.9em;
}

.demo-section {
  margin-bottom: 3rem;
  padding: 1.5rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0066cc;
  border-radius: 4px;
}

.section-toggle {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  font: inherit;
  font-size: 24px;
  font-weight: bold;
  color: #0066cc;
  text-align: left;
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 20px;
}

.section-toggle:hover {
  text-decoration: underline;
}

.section-toggle:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 4px;
  border-radius: 4px;
}

.toggle-icon {
  display: inline-block;
  width: 16px;
  text-align: center;
  transition: transform 0.2s ease;
}

.collapsible-content {
  animation: slideDown 0.3s ease;
}

@keyframes slideDown {
  from {
    opacity: 0;
    max-height: 0;
  }
  to {
    opacity: 1;
    max-height: 2000px;
  }
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
