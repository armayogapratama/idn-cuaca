<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])

const searchTerm = reactive({
  query: '',
  timeout: null,
  result: null
})

const handleSearach = () => {
  clearTimeout(searchTerm.timeout)
  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      const res = await fetch(
        `http://api.weatherapi.com/v1/search.json?key=7536a5734ade461f8f1100034240103&q=${searchTerm.query}`
      )

      const data = await res.json()
      searchTerm.result = data
    } else {
      searchTerm.result = null
    }
  }, 300)
}

const handleWheater = async (id) => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=7536a5734ade461f8f1100034240103&q=id:${id}&days=3&aqi=no&alerts=no`
  )

  const data = await res.json()
  emit('place-data', data)
  searchTerm.query = ''
  searchTerm.result = null
}
</script>

<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="handleSearach"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchTerm.result !== null">
        <div v-for="place in searchTerm.result" :key="place.id">
          <button
            @click="handleWheater(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ place.name }}, {{ place.region }}, {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
