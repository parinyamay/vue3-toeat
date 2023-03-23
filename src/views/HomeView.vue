<script setup lang="ts">
import { ref } from 'vue'

interface Restaurant {
  name: string
  status: RestaurantStatus
  dishes: Dish[]
}

type RestaurantStatus = 'Want to Try' | 'Recommended' | 'Do Not Recommend' | 'Must Try'

const restaurantList = ref<Restaurant[]>([])
const newRestaurant = ref<Restaurant>({})

// How to extract valuoe from type
const statusList = ['Want to Try', 'Recommended', 'Do Not Recommend', 'Must Try']

function addRestaurant() {
  restaurantList.value.push({
    name: newRestaurant.value.name,
    status: newRestaurant.value.status,
    dishes: []
  })
}
</script>

<template>
  <main>
    <pre>
      {{ newRestaurant }}
    </pre>
    <!-- Create a form that allows users to add a restaurant to a list. -->
    <form @submit.prevent="addRestaurant">
      <div>
        <label for="restaurant-name">Restaurant Name</label>
        <input id="restaurant-name" v-model="newRestaurant.name" type="text" />
      </div>

      <div>
        <label for="restaurant-address">Restaurant Status</label>
        <select name="restaurant-status" id="restaurant-status" v-model="newRestaurant.status">
          <option v-for="status in statusList" :key="status" :value="status">
            {{ status }}
          </option>
        </select>
      </div>

      <button type="submit">Add Restaurant</button>
    </form>

    <ul>
      <li v-for="restaurant in restaurantList" :key="restaurant.name">
        {{ restaurant.name }}
      </li>
    </ul>
  </main>
</template>
