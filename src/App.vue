<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <div class="brand">ToEat</div>
      <nav>
        <RouterLink to="/">Restaurants</RouterLink>
        <RouterLink to="/dishes">Dishes</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  background: var(--color-background-soft);
  border-bottom: 1px solid var(--color-border);
  box-shadow: var(--shadow-sm);
  position: sticky;
  top: 0;
  z-index: 100;
  margin-bottom: 2rem;
}

.wrapper {
  max-width: 960px;
  margin: 0 auto;
  padding: 0 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 56px;
}

.brand {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--color-primary);
  letter-spacing: -0.02em;
}

nav {
  display: flex;
  gap: 0.25rem;
}

nav a {
  display: inline-flex;
  align-items: center;
  padding: 0.4rem 1rem;
  border-radius: var(--radius-sm);
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--color-text-secondary);
  transition: all 0.2s;
}

nav a:hover {
  color: var(--color-primary);
  background: var(--color-primary-light);
}

nav a.router-link-exact-active {
  color: var(--color-primary);
  background: var(--color-primary-light);
  font-weight: 600;
}
</style>
