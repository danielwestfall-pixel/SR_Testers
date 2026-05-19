<template>
  <div class="demo-page">
    <header class="demo-header">
      <h1>Accessible Component Library</h1>
      <p>Testing suite for accessibility-first Vue components</p>
    </header>

    <nav class="demo-nav">
      <ul>
        <li><a href="#click-to-reveal">Click to Reveal</a></li>
        <li><a href="#tabs">Tabs</a></li>
        <li><a href="#paginator">Content Paginator</a></li>
      </ul>
    </nav>

    <main class="demo-content">
      <section id="click-to-reveal" class="component-demo">
        <ClickToRevealButtonDemo />
      </section>
      <section id="tabs" class="component-demo">
        <TabsDemo />
      </section>
      <section id="paginator" class="component-demo">
        <ContentPaginatorDemo />
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import ClickToRevealButtonDemo from './ClickToRevealButtonDemo.vue'
import TabsDemo from './TabsDemo.vue'
import ContentPaginatorDemo from './ContentPaginatorDemo.vue'
</script>

<style scoped>
.demo-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.demo-header {
  background: rgba(0, 0, 0, 0.1);
  color: white;
  padding: 3rem 2rem;
  text-align: center;
}

.demo-header h1 {
  margin: 0;
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.demo-header p {
  margin: 0;
  font-size: 1.1rem;
  opacity: 0.9;
}

.demo-nav {
  background: white;
  padding: 1.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  sticky: true;
  position: sticky;
  top: 0;
  z-index: 100;
}

.demo-nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  gap: 2rem;
}

.demo-nav a {
  color: #0066cc;
  text-decoration: none;
  font-weight: 500;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.demo-nav a:hover {
  background-color: #f0f8ff;
}

.demo-nav a:focus {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

.demo-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.component-demo {
  background: white;
  border-radius: 8px;
  padding: 2rem;
  margin-bottom: 3rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.component-demo + .component-demo {
  margin-top: 3rem;
}
</style>
