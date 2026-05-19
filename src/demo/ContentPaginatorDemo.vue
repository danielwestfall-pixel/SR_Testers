<template>
  <div class="paginator-demo">
    <h2>Content Paginator Component</h2>
    <p>
      A slide-deck-style pagination component with an inset content window. Navigate using the
      large arrow buttons positioned at the sides or use keyboard arrows for accessibility.
    </p>

    <!-- Full Page Example -->
    <h3>Example: Full Page Paginator</h3>
    <div class="paginator-wrapper">
      <header class="page-header">
        <h1>Slide Deck Example</h1>
        <p>Navigate through the slides using the arrow buttons</p>
      </header>

      <ContentPaginatorContainer :pages="slidePages">
        <div class="paginator-layout">
          <PaginatedContent>
            <template #default="{ page }">
              <div class="slide-content">
                <h2>{{ page.title }}</h2>
                <p>{{ page.content }}</p>
              </div>
            </template>
          </PaginatedContent>
          <NavigationArrows />
        </div>
      </ContentPaginatorContainer>

      <footer class="page-footer">
        <p>&copy; 2026 Accessible Component Library - Keyboard and Screen Reader Friendly</p>
      </footer>
    </div>

    <!-- Accessibility Features -->
    <h3>Accessibility Features</h3>
    <ul>
      <li>
        <strong>ARIA Labels:</strong> Navigation buttons have descriptive aria-labels for screen
        readers
      </li>
      <li>
        <strong>Keyboard Navigation:</strong> Navigate pages using Tab to reach buttons, then
        Enter or Space to activate
      </li>
      <li>
        <strong>Disabled State:</strong> Arrow buttons disable when at first or last page,
        preventing out-of-bounds navigation
      </li>
      <li>
        <strong>Visual Feedback:</strong> Current page indicator shows position in the sequence
      </li>
      <li>
        <strong>Focus Management:</strong> Clear focus indicators on navigation buttons
      </li>
      <li>
        <strong>Semantic Structure:</strong> Proper button semantics with meaningful labels
      </li>
    </ul>

    <!-- How to Use -->
    <h3>How to Use</h3>

    <h4>1. Import</h4>
    <div class="code-block" v-pre>
      <pre><code>import {
  ContentPaginatorContainer,
  PaginatedContent,
  NavigationArrows,
  type PaginatedPage
} from './components'</code></pre>
    </div>

    <h4>2. Define Pages</h4>
    <div class="code-block" v-pre>
      <pre><code>const pages: PaginatedPage[] = [
  {
    id: 'page1',
    title: 'Welcome',
    content: 'This is the first page of content...'
  },
  {
    id: 'page2',
    title: 'Features',
    content: 'This page describes the key features...'
  },
  {
    id: 'page3',
    title: 'More Info',
    content: 'Additional content goes here...'
  }
]</code></pre>
    </div>

    <h4>3. Basic Usage</h4>
    <div class="code-block" v-pre>
      <pre><code>&lt;template&gt;
  &lt;div class="app-layout"&gt;
    &lt;header&gt;
      &lt;h1&gt;My App&lt;/h1&gt;
    &lt;/header&gt;

    &lt;ContentPaginatorContainer :pages="pages"&gt;
      &lt;div class="paginator-layout"&gt;
        &lt;PaginatedContent /&gt;
        &lt;NavigationArrows /&gt;
      &lt;/div&gt;
    &lt;/ContentPaginatorContainer&gt;

    &lt;footer&gt;
      &lt;p&gt;Footer content&lt;/p&gt;
    &lt;/footer&gt;
  &lt;/div&gt;
&lt;/template&gt;</code></pre>
    </div>

    <h4>4. Custom Page Content with Slots</h4>
    <div class="code-block" v-pre>
      <pre><code>&lt;PaginatedContent&gt;
  &lt;template #default="{ page }"&gt;
    &lt;div class="custom-slide"&gt;
      &lt;h2&gt;{{ page.title }}&lt;/h2&gt;
      &lt;img :src="page.imageUrl" :alt="page.title" /&gt;
      &lt;p&gt;{{ page.content }}&lt;/p&gt;
    &lt;/div&gt;
  &lt;/template&gt;
&lt;/PaginatedContent&gt;</code></pre>
    </div>

    <!-- Props Table -->
    <h4>ContentPaginatorContainer Props</h4>
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
          <td><code>pages</code></td>
          <td>PaginatedPage[]</td>
          <td>required</td>
          <td>Array of page objects with id, title, and content</td>
        </tr>
        <tr>
          <td><code>defaultPageId</code></td>
          <td>string</td>
          <td>First page</td>
          <td>ID of the page to show on mount</td>
        </tr>
      </tbody>
    </table>

    <!-- How It Works -->
    <h4>How It Works</h4>
    <ul>
      <li>
        <strong>Container State:</strong> ContentPaginatorContainer manages the current page index
        and provides navigation methods via provide/inject
      </li>
      <li>
        <strong>Page Display:</strong> PaginatedContent renders the current page with a fade-in
        animation
      </li>
      <li>
        <strong>Navigation:</strong> NavigationArrows provides left/right buttons positioned at
        50% height for easy access
      </li>
      <li>
        <strong>Disabled State:</strong> Arrow buttons automatically disable when at boundaries
        (first/last page)
      </li>
      <li>
        <strong>Flexible Layout:</strong> Static header and footer remain visible while center
        content paginate
      </li>
      <li>
        <strong>Page Counter:</strong> Displays current page number (e.g., "2 / 5")
      </li>
    </ul>

    <!-- Accessibility Implementation -->
    <h4>Accessibility Implementation</h4>
    <div class="code-block" v-pre>
      <pre><code>// Navigation buttons structure
&lt;button
  class="arrow-button"
  :disabled="!canGoPrev()"
  aria-label="Previous page"
  @click="goPrev"
&gt;
  &lt;span aria-hidden="true"&gt;‹&lt;/span&gt;
&lt;/button&gt;

// Page content with semantic structure
&lt;div class="page-content"&gt;
  &lt;h2&gt;{{ page.title }}&lt;/h2&gt;
  &lt;p&gt;{{ page.content }}&lt;/p&gt;
&lt;/div&gt;

// Page counter for context
&lt;div class="page-counter"&gt;
  {{ currentPage + 1 }} / {{ totalPages }}
&lt;/div&gt;</code></pre>
    </div>
  </div>
</template>

<script setup lang="ts">
import {
  ContentPaginatorContainer,
  PaginatedContent,
  NavigationArrows,
  type PaginatedPage
} from '../components'

const slidePages: PaginatedPage[] = [
  {
    id: 'slide1',
    title: 'Welcome to the Presentation',
    content: 'This is a fully accessible slide deck component. Use the arrow buttons or keyboard to navigate between pages. The header and footer stay visible while the content paginate in the center.'
  },
  {
    id: 'slide2',
    title: 'Key Features',
    content: 'Navigate with large circular arrow buttons positioned at the sides. The buttons are fully keyboard accessible and properly labeled for screen readers.'
  },
  {
    id: 'slide3',
    title: 'Accessibility First',
    content: 'Every button has an aria-label, disabled states are semantic, and focus is clearly visible. Perfect for testing keyboard navigation and screen reader support.'
  },
  {
    id: 'slide4',
    title: 'Flexible Layout',
    content: 'The header and footer remain static while the center content area paginate. This creates a professional slide deck appearance with proper document structure.'
  },
  {
    id: 'slide5',
    title: 'Ready to Use',
    content: 'You can now use this component in your projects. Navigate with arrow buttons or Tab to focus and Enter/Space to activate. This is the final slide!'
  }
]
</script>

<style scoped>
.paginator-demo {
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

/* Paginator wrapper and layout */
.paginator-wrapper {
  display: flex;
  flex-direction: column;
  height: 600px;
  margin: 20px 0;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: white;
}

.page-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 30px;
  text-align: center;
  flex-shrink: 0;
}

.page-header h1 {
  margin: 0 0 10px 0;
  font-size: 28px;
}

.page-header p {
  margin: 0;
  color: rgba(255, 255, 255, 0.9);
  font-size: 14px;
}

.paginator-layout {
  flex: 1;
  position: relative;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.slide-content {
  text-align: center;
}

.slide-content h2 {
  margin-bottom: 20px;
}

.slide-content p {
  margin: 0;
  font-size: 18px;
  max-width: 600px;
}

.page-footer {
  background-color: #f5f5f5;
  border-top: 1px solid #e0e0e0;
  padding: 20px;
  text-align: center;
  flex-shrink: 0;
}

.page-footer p {
  margin: 0;
  color: #999;
  font-size: 12px;
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
</style>
