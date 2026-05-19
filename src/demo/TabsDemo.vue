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
        <TabPanels />
      </div>
    </TabsContainer>

    <!-- Example 2: Tabs Below Panel -->
    <h3>Example 2: Tabs Below Content</h3>
    <TabsContainer :tabs="sampleTabs" default-tab-id="features">
      <div class="tabs-layout-below">
        <TabPanels />
        <TabsBar aria-label="Sample tabs" />
      </div>
    </TabsContainer>

    <!-- Example 3: Custom Content with Slots -->
    <h3>Example 3: Custom Panel Content</h3>
    <TabsContainer :tabs="customTabs">
      <div class="tabs-layout-above">
        <TabsBar aria-label="Custom tabs" />
        <TabPanels>
          <template #panel-icons="{ tab }">
            <div class="custom-content">
              <p>{{ tab.content }}</p>
              <div class="icon-grid">
                <span v-for="icon in ['🎨', '⚙️', '🔧']" :key="icon" class="icon">
                  {{ icon }}
                </span>
              </div>
            </div>
          </template>
          <template #panel-code="{ tab }">
            <div class="custom-content">
              <p>{{ tab.content }}</p>
              <pre><code>&lt;TabsContainer :tabs="data"&gt;
  &lt;TabsBar /&gt;
  &lt;TabPanels /&gt;
&lt;/TabsContainer&gt;</code></pre>
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
import { TabsContainer, TabsBar, TabPanels, type TabItem } from '../../components'

const sampleTabs: TabItem[] = [
  {
    id: 'overview',
    label: 'Overview',
    content:
      'This is the overview tab. Tabs can be positioned above or below the content area. Use arrow keys to navigate between tabs.'
  },
  {
    id: 'features',
    label: 'Features',
    content:
      'Features tab content. The component uses ARIA attributes and semantic HTML for full accessibility and screen reader support.'
  },
  {
    id: 'settings',
    label: 'Settings',
    content: 'Settings tab content. Keyboard navigation includes Home, End, and arrow keys.'
  },
  {
    id: 'disabled',
    label: 'Disabled Tab',
    content: 'This tab cannot be selected',
    disabled: true
  }
]

const customTabs: TabItem[] = [
  {
    id: 'icons',
    label: 'Visual',
    content: 'This tab demonstrates custom slot content with icons'
  },
  {
    id: 'code',
    label: 'Code',
    content: 'This tab shows code examples'
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
  flex-direction: column-reverse;
  margin: 20px 0;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
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

/* Custom content demo */
.custom-content {
  padding: 15px;
}

.icon-grid {
  display: flex;
  gap: 15px;
  margin-top: 15px;
}

.icon {
  font-size: 32px;
  cursor: pointer;
  transition: transform 0.2s;
}

.icon:hover {
  transform: scale(1.2);
}

.custom-content pre {
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
  margin: 10px 0;
}

.custom-content code {
  font-family: 'Courier New', monospace;
  font-size: 12px;
  color: #333;
}
</style>
