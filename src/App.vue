<template>
  <div id="app">
    <DemoPage />
  </div>
</template>

<script setup lang="ts">
import DemoPage from './demo/DemoPage.vue'
</script>

<style>
#app {
  width: 100%;
  height: 100%;
}

/* Shared Demo Styles */
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
</style>
