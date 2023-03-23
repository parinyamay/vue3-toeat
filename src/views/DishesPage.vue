<script setup lang="ts">
import { ref } from 'vue'

const restaurantStatusList = ['Want to Try', 'Recommended', 'Do Not Recommend', 'Must Try'] as const

type RecommendStatus = (typeof restaurantStatusList)[number]

interface Dish {
  name?: string
  diet?: Diet
  status?: RecommendStatus
}

type Diet = 'vegetarian' | 'vegan' | 'gluten-free' | 'pescatarian' | 'lactose-free' | 'other'

// How to extract valuoe from type
const statusList = ['Want to Try', 'Recommended', 'Do Not Recommend', 'Must Try']

const dishList = ref<Dish[]>([])
const newDish = ref<Dish>({
  status: 'Want to Try'
})

function addDish() {
  dishList.value.push({
    name: newDish.value.name,
    status: newDish.value.status
  })
}
</script>

<template>
  <main>
    <pre>
      {{ newDish }}
    </pre>
    <!-- Create a form that allows users to add a restaurant to a list. -->
    <form @submit.prevent="addDish">
      <div>
        <label for="dish-name">Dish Name</label>
        <input id="dish-name" v-model="newDish.name" type="text" />
      </div>

      <div>
        <label for="dish-address">Dish Status</label>
        <select name="dish-status" id="dish-status" v-model="newDish.status">
          <option v-for="status in statusList" :key="status" :value="status">
            {{ status }}
          </option>
        </select>
      </div>

      <button type="submit">Add Dish</button>
    </form>

    <ul>
      <li v-for="dish in dishList" :key="dish.name">{{ dish.name }} - {{ dish.status }}</li>
    </ul>
  </main>
</template>
