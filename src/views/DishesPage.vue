<script setup lang="ts">
import { ref } from 'vue'
import type { Dish, RecommendStatus } from '../type'
import { restaurantStatusList } from '../constants'

interface DishWithId extends Dish {
  id: number
}

let nextDishId = 0
const dishList = ref<DishWithId[]>([])
const newDish = ref<Dish>({
  status: 'Want to Try'
})

function addDish() {
  dishList.value.push({
    id: nextDishId++,
    name: newDish.value.name,
    status: newDish.value.status
  })
  newDish.value = { status: 'Want to Try' }
}

function removeDish(index: number) {
  dishList.value.splice(index, 1)
}

function statusClass(status?: RecommendStatus) {
  switch (status) {
    case 'Recommended':
      return 'badge-success'
    case 'Must Try':
      return 'badge-info'
    case 'Do Not Recommend':
      return 'badge-danger'
    default:
      return 'badge-warning'
  }
}
</script>

<template>
  <main class="page">
    <section class="card form-card">
      <h2 class="card-title">Add Dish</h2>
      <form @submit.prevent="addDish" class="form">
        <div class="form-group">
          <label for="dish-name">Name</label>
          <input
            id="dish-name"
            v-model="newDish.name"
            type="text"
            placeholder="Enter dish name"
          />
        </div>

        <div class="form-group">
          <label for="dish-status">Status</label>
          <select id="dish-status" v-model="newDish.status">
            <option v-for="status in restaurantStatusList" :key="status" :value="status">
              {{ status }}
            </option>
          </select>
        </div>

        <button type="submit" class="btn-primary">Add Dish</button>
      </form>
    </section>

    <section class="list-section">
      <h2 class="section-title">
        Dishes
        <span v-if="dishList.length" class="count">{{ dishList.length }}</span>
      </h2>

      <p v-if="!dishList.length" class="empty-state">No dishes yet. Add one above to get started.</p>

      <TransitionGroup name="list" tag="ul" class="item-list">
        <li v-for="(dish, index) in dishList" :key="dish.id" class="item-card">
          <div class="item-content">
            <span class="item-name">{{ dish.name }}</span>
            <span :class="['badge', statusClass(dish.status)]">{{ dish.status }}</span>
          </div>
          <button class="btn-remove" @click="removeDish(index)" aria-label="Remove">&times;</button>
        </li>
      </TransitionGroup>
    </section>
  </main>
</template>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.card {
  background: var(--color-background-soft);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  padding: 1.5rem;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-heading);
  margin-bottom: 1.25rem;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.375rem;
}

.form-group label {
  font-size: 0.8125rem;
  font-weight: 600;
  color: var(--color-text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.form-group input,
.form-group select {
  padding: 0.625rem 0.875rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-background);
  color: var(--color-text);
  font-size: 0.9375rem;
  transition: all 0.2s;
  outline: none;
}

.form-group input::placeholder {
  color: var(--color-text-secondary);
  opacity: 0.6;
}

.form-group input:focus,
.form-group select:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px var(--color-primary-light);
}

.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.625rem 1.25rem;
  background: var(--color-primary);
  color: #fff;
  border: none;
  border-radius: var(--radius-md);
  font-size: 0.9375rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  margin-top: 0.25rem;
}

.btn-primary:hover {
  background: var(--color-primary-hover);
  box-shadow: var(--shadow-md);
  transform: translateY(-1px);
}

.btn-primary:active {
  transform: translateY(0);
}

.section-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-heading);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.count {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 1.5rem;
  height: 1.5rem;
  padding: 0 0.5rem;
  font-size: 0.75rem;
  font-weight: 700;
  background: var(--color-primary-light);
  color: var(--color-primary);
  border-radius: 999px;
}

.empty-state {
  text-align: center;
  color: var(--color-text-secondary);
  padding: 2rem;
  font-size: 0.9375rem;
}

.item-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.item-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.875rem 1rem;
  background: var(--color-background-soft);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  transition: all 0.2s;
}

.item-card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--color-border-hover);
}

.item-content {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
  min-width: 0;
}

.item-name {
  font-weight: 500;
  color: var(--color-heading);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.badge {
  display: inline-flex;
  align-items: center;
  padding: 0.2rem 0.625rem;
  border-radius: 999px;
  font-size: 0.75rem;
  font-weight: 600;
  white-space: nowrap;
  flex-shrink: 0;
}

.badge-success {
  background: rgba(16, 185, 129, 0.1);
  color: #059669;
}

.badge-info {
  background: rgba(59, 130, 246, 0.1);
  color: #2563eb;
}

.badge-warning {
  background: rgba(245, 158, 11, 0.1);
  color: #d97706;
}

.badge-danger {
  background: rgba(239, 68, 68, 0.1);
  color: #dc2626;
}

.btn-remove {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.75rem;
  height: 1.75rem;
  border: none;
  background: transparent;
  color: var(--color-text-secondary);
  border-radius: var(--radius-sm);
  font-size: 1.125rem;
  cursor: pointer;
  transition: all 0.15s;
  flex-shrink: 0;
  margin-left: 0.5rem;
}

.btn-remove:hover {
  background: rgba(239, 68, 68, 0.1);
  color: var(--color-danger);
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(20px);
}

@media (min-width: 640px) {
  .form {
    flex-direction: row;
    align-items: flex-end;
  }

  .form-group {
    flex: 1;
  }

  .btn-primary {
    margin-top: 0;
    align-self: flex-end;
  }
}
</style>
