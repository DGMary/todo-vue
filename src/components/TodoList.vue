<script setup lang="ts">
import { ref } from 'vue'

const loading = ref(false)
const post = ref(null)
const error = ref(null)

async function getData() {
  const url = 'https://jsonplaceholder.typicode.com/users/1/todos'
  loading.value = true
  try {
    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`)
    }

    const json = await response.json()
    post.value = json
    console.log('json', json)
  } catch (error) {
    console.error(error.message)
  } finally {
    loading.value = false
  }
}
getData()
</script>

<template>
  <div class="post">
    <div v-if="loading" class="loading">Loading...</div>

    <div v-if="error" class="error">{{ error }}</div>

    <ul v-if="post">
      <li v-for="item in post" :key="item.id" :class="{ active: item.completed }">
        <span> {{ item.title }} </span>
        <button v-if="item.completed" class="item-fav">Completed</button>
      </li>
    </ul>
  </div>
</template>
<style>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin-top: 24px;
}
li {
  border: 1px solid lightgrey;
  padding: 8px;
  margin-bottom: 8px;
  display: flex;
  justify-content: space-between;
}
.item-fav {
  color: palevioletred;
  font-weight: 700;
  text-transform: uppercase;
  display: inline-flex;
  margin-left: 8px;
  background: transparent;
  cursor: pointer;
}
li.active {
  border-color: palevioletred;
}
</style>
