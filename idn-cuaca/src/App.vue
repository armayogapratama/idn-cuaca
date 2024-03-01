<script setup>
import { ref } from 'vue'
import SearchPage from './components/SearchPage/SearchPage.vue'
import WheaterCards from './components/WheaterCards/WheaterCards.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const removePlace = (name) => {
  places.value = places.value.filter((place) => place.location.name !== name)
}
</script>

<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('id-ID', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>

    <!-- Search -->
    <div>
      <SearchPage @place-data="addPlace" />
    </div>

    <!-- Wheater Cards -->
    <div class="grid grid-cols-3 gap-4">
      <div v-for="(place, index) in places" :key="index">
        <WheaterCards :place="place" @remove-place="removePlace" />
      </div>
    </div>
  </main>
</template>
