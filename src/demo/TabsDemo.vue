<template>
  <div class="tabs-demo">
    <h2>Tabs Component</h2>
    <p>
      An accessible, arrangeable tab component system. Tabs and panels can be
      positioned independently - tabs can appear above or below the content.
    </p>

    <!-- Example 1: Tabs Above Panel -->
    <h3>Example 1: Tabs Above Content</h3>
    <TabsContainer :tabs="sampleTabs">
      <div class="tabs-layout-above">
        <TabsBar aria-label="Sample tabs" />
        <TabPanels>
          <template #panel-example1-overview>
            <div class="tab-content-interactive">
              <p>This is the overview tab. Use Tab to navigate through the interactive elements below.</p>
              <button class="demo-button">Click me</button>
              <a href="#" class="demo-link">Test link</a>
              <input type="text" placeholder="Test input field" class="demo-input" />
            </div>
          </template>
          <template #panel-example1-features>
            <div class="tab-content-interactive">
              <p>Features tab with interactive content for keyboard testing.</p>
              <button class="demo-button">Action Button</button>
              <button class="demo-button">Another Button</button>
              <a href="#" class="demo-link">Documentation</a>
            </div>
          </template>
          <template #panel-example1-settings>
            <div class="tab-content-interactive">
              <p>Settings tab - test form controls with keyboard.</p>
              <label>
                <input type="checkbox" /> Enable feature
              </label>
              <label>
                <input type="radio" name="option" /> Option 1
              </label>
              <label>
                <input type="radio" name="option" /> Option 2
              </label>
              <button class="demo-button">Save Settings</button>
            </div>
          </template>
        </TabPanels>
      </div>
    </TabsContainer>

    <!-- Example 2: Tabs Below Panel -->
    <h3>Example 2: Tabs Below Content</h3>
    <TabsContainer :tabs="sampleTabsExample2" default-tab-id="example2-features">
      <div class="tabs-layout-below">
        <TabsBar aria-label="Sample tabs" />
        <TabPanels>
          <template #panel-example2-overview>
            <div class="tab-content-interactive">
              <p>Overview content with interactive elements.</p>
              <button class="demo-button">Primary Button</button>
              <input type="text" placeholder="Text input" class="demo-input" />
            </div>
          </template>
          <template #panel-example2-features>
            <div class="tab-content-interactive">
              <p>Features tab - tabs are positioned below but keyboard access is optimized.</p>
              <select class="demo-select">
                <option>Select an option</option>
                <option>Option A</option>
                <option>Option B</option>
              </select>
              <button class="demo-button">Submit</button>
            </div>
          </template>
          <template #panel-example2-settings>
            <div class="tab-content-interactive">
              <p>Settings with form elements for keyboard testing.</p>
              <textarea class="demo-textarea" placeholder="Enter text here..."></textarea>
              <button class="demo-button">Reset</button>
              <a href="#" class="demo-link">Learn more</a>
            </div>
          </template>
        </TabPanels>
      </div>
    </TabsContainer>

    <!-- Accessibility Features -->
    <h3>Accessibility Features</h3>
    <ul>
      <li>
        <strong>ARIA Attributes:</strong> Proper roles (tab, tablist, tabpanel),
        aria-selected, aria-controls, and aria-labelledby for semantic structure
      </li>
      <li>
        <strong>Keyboard Navigation:</strong> Arrow keys, Home/End to navigate
        tabs; Tab key to move focus in and out of the component
      </li>
      <li>
        <strong>Screen Reader Support:</strong> Tab state announced automatically;
        panel labels linked to their tabs
      </li>
      <li>
        <strong>Focus Management:</strong> Clear focus indicators; focus
        automatically moves to new tab when selected via keyboard
      </li>
      <li>
        <strong>Disabled Tabs:</strong> Full accessibility for disabled state
        with reduced opacity and cursor
      </li>
    </ul>

    <!-- How to Use -->
    <h3>How to Use</h3>

    <h4>1. Import</h4>
    <div class="code-block" v-pre>
      <pre><code>import { TabsContainer, TabsBar, TabPanels, type TabItem } from './components'</code></pre>
    </div>

    <h4>2. Define Tab Data</h4>
    <div class="code-block" v-pre>
      <pre><code>const tabs: TabItem[] = [
  { id: 'overview', label: 'Overview', content: 'Overview content...' },
  { id: 'features', label: 'Features', content: 'Features content...' },
  { id: 'settings', label: 'Settings', content: 'Settings content...', disabled: false }
]</code></pre>
    </div>

    <h4>3. Basic Usage - Tabs Above</h4>
    <div class="code-block" v-pre>
      <pre><code>&lt;TabsContainer :tabs="tabs"&gt;
  &lt;div class="tabs-layout-above"&gt;
    &lt;TabsBar aria-label="Main tabs" /&gt;
    &lt;TabPanels /&gt;
  &lt;/div&gt;
&lt;/TabsContainer&gt;</code></pre>
    </div>

    <h4>4. Tabs Below Content</h4>
    <div class="code-block" v-pre>
      <pre><code>&lt;TabsContainer :tabs="tabs"&gt;
  &lt;div class="tabs-layout-below"&gt;
    &lt;TabPanels /&gt;
    &lt;TabsBar aria-label="Main tabs" /&gt;
  &lt;/div&gt;
&lt;/TabsContainer&gt;</code></pre>
    </div>

    <h4>5. Custom Content with Slots</h4>
    <div class="code-block" v-pre>
      <pre><code>&lt;TabPanels&gt;
  &lt;template #panel-custom="{ tab }"&gt;
    &lt;div&gt;Custom HTML for {{ tab.label }}&lt;/div&gt;
  &lt;/template&gt;
&lt;/TabPanels&gt;</code></pre>
    </div>

    <!-- Props Table -->
    <h4>TabsContainer Props</h4>
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
          <td><code>tabs</code></td>
          <td>TabItem[]</td>
          <td>required</td>
          <td>Array of tab objects with id, label, content, and optional disabled</td>
        </tr>
        <tr>
          <td><code>defaultTabId</code></td>
          <td>string</td>
          <td>First tab</td>
          <td>ID of the tab to show on mount</td>
        </tr>
      </tbody>
    </table>

    <h4>TabsBar Props</h4>
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
          <td><code>ariaLabel</code></td>
          <td>string</td>
          <td>"Tabs"</td>
          <td>Accessible label for the tab list</td>
        </tr>
      </tbody>
    </table>

    <!-- How It Works -->
    <h4>How It Works</h4>
    <ul>
      <li>
        <strong>State Management:</strong> TabsContainer manages active tab state
        using Vue's ref() and provides it to child components via provide/inject
      </li>
      <li>
        <strong>Tab Selection:</strong> TabsBar renders all tabs and handles click
        and keyboard events
      </li>
      <li>
        <strong>Panel Rendering:</strong> TabPanels renders all panels but only
        displays the active one using CSS display
      </li>
      <li>
        <strong>Flexible Layout:</strong> Components can be arranged in any DOM
        order using CSS Grid or Flexbox in parent container
      </li>
      <li>
        <strong>Keyboard Navigation:</strong> Arrow keys move between tabs; Home/End
        jump to first/last tab; Tab key moves focus out of component
      </li>
      <li>
        <strong>Focus Management:</strong> When tab selection changes via keyboard,
        focus automatically moves to the new tab button
      </li>
    </ul>

    <!-- Accessibility Implementation -->
    <h4>Accessibility Implementation</h4>
    <div class="code-block" v-pre>
      <pre><code>// TabsBar element structure
&lt;div class="tabs-bar" role="tablist" aria-label="Tabs"&gt;
  &lt;button
    v-for="tab in tabs"
    role="tab"
    :id="`tab-${tab.id}`"
    :aria-selected="isActive"
    :aria-controls="`panel-${tab.id}`"
    :disabled="tab.disabled"
  &gt;
    {{ tab.label }}
  &lt;/button&gt;
&lt;/div&gt;

// TabPanels element structure
&lt;div
  :id="`panel-${tab.id}`"
  role="tabpanel"
  :aria-labelledby="`tab-${tab.id}`"
&gt;
  {{ tab.content }}
&lt;/div&gt;</code></pre>
    </div>
  </div>
</template>

<script setup lang="ts">
import { TabsContainer, TabsBar, TabPanels, type TabItem } from '../components'

const sampleTabs: TabItem[] = [
  {
    id: 'example1-overview',
    label: 'Overview',
    content:
      'This is the overview tab. Tabs can be positioned above or below the content area. Use arrow keys to navigate between tabs.'
  },
  {
    id: 'example1-features',
    label: 'Features',
    content:
      'Features tab content. The component uses ARIA attributes and semantic HTML for full accessibility and screen reader support.'
  },
  {
    id: 'example1-settings',
    label: 'Settings',
    content: 'Settings tab content. Keyboard navigation includes Home, End, and arrow keys.'
  },
  {
    id: 'example1-disabled',
    label: 'Disabled Tab',
    content: 'This tab cannot be selected',
    disabled: true
  }
]

const sampleTabsExample2: TabItem[] = [
  {
    id: 'example2-overview',
    label: 'Overview',
    content:
      'This is the overview tab. Tabs can be positioned above or below the content area. Use arrow keys to navigate between tabs.'
  },
  {
    id: 'example2-features',
    label: 'Features',
    content:
      'Features tab content. The component uses ARIA attributes and semantic HTML for full accessibility and screen reader support.'
  },
  {
    id: 'example2-settings',
    label: 'Settings',
    content: 'Settings tab content. Keyboard navigation includes Home, End, and arrow keys.'
  },
  {
    id: 'example2-disabled',
    label: 'Disabled Tab',
    content: 'This tab cannot be selected',
    disabled: true
  }
]
</script>

<style scoped>
.tabs-demo {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}

h2 {
  color: #0066cc;
  margin-top: 0;
  margin-bottom: 10px;
}

h3 {
  color: #0066cc;
  margin-top: 40px;
  margin-bottom: 15px;
  border-bottom: 2px solid #e0e0e0;
  padding-bottom: 10px;
}

h4 {
  color: #333;
  margin-top: 25px;
  margin-bottom: 15px;
}

p {
  color: #666;
  line-height: 1.6;
}

ul {
  color: #666;
  line-height: 1.8;
}

li {
  margin-bottom: 10px;
}

/* Layout examples */
.tabs-layout-above {
  display: flex;
  flex-direction: column;
  margin: 20px 0;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.tabs-layout-below {
  display: flex;
  flex-direction: column;
  margin: 20px 0;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.tabs-layout-below > :first-child {
  order: 2;
}

.tabs-layout-below > :last-child {
  order: 1;
}

/* Code block styling */
.code-block {
  background-color: #2d2d2d;
  color: #f8f8f2;
  padding: 15px;
  border-radius: 4px;
  overflow-x: auto;
  margin: 10px 0;
}

.code-block pre {
  margin: 0;
  font-family: 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.5;
}

.code-block code {
  color: #f8f8f2;
}

/* Props table styling */
.props-table {
  width: 100%;
  border-collapse: collapse;
  margin: 15px 0;
  border: 1px solid #e0e0e0;
}

.props-table thead {
  background-color: #0066cc;
  color: white;
}

.props-table th,
.props-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #e0e0e0;
}

.props-table tbody tr:hover {
  background-color: #f5f5f5;
}

.props-table code {
  background-color: #f5f5f5;
  padding: 2px 6px;
  border-radius: 3px;
  font-family: 'Courier New', monospace;
  color: #666;
}

/* Interactive content in tabs */
.tab-content-interactive {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.tab-content-interactive p {
  margin: 0;
  color: #555;
  line-height: 1.6;
}

.demo-button {
  padding: 10px 16px;
  background-color: #0066cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: background-color 0.2s;
  align-self: flex-start;
}

.demo-button:hover {
  background-color: #0052a3;
}

.demo-button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.demo-button:active {
  background-color: #003d7a;
}

.demo-link {
  color: #0066cc;
  text-decoration: none;
  font-weight: 500;
  align-self: flex-start;
}

.demo-link:hover {
  text-decoration: underline;
}

.demo-link:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.demo-input,
.demo-select,
.demo-textarea {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-family: inherit;
  font-size: 14px;
  max-width: 300px;
}

.demo-input:focus,
.demo-select:focus,
.demo-textarea:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
  border-color: #0066cc;
}

.demo-textarea {
  min-height: 100px;
  resize: vertical;
  max-width: 100%;
}

.tab-content-interactive label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  color: #555;
  font-size: 14px;
}

.tab-content-interactive input[type="checkbox"],
.tab-content-interactive input[type="radio"] {
  cursor: pointer;
}

.tab-content-interactive input[type="checkbox"]:focus-visible,
.tab-content-interactive input[type="radio"]:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}
</style>
