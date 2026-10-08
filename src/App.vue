<script setup>
import { ref, onMounted } from 'vue'
import FrogItem from './components/FrogItem.vue'

const frogs = ref([])

const newName = ref('')
const newColor = ref('')
const newPlace = ref('')
const newRarity = ref('Common')

function addFrog() {
  if (
    newName.value.trim() === '' ||
    newColor.value.trim() === '' ||
    newPlace.value.trim() === ''
  ) {
    return
  }

  frogs.value.push({
    id: Date.now(),
    name: newName.value,
    color: newColor.value,
    place: newPlace.value,
    rarity: newRarity.value
  })

  newName.value = ''
  newColor.value = ''
  newPlace.value = ''
  newRarity.value = 'Common'

  saveFrogs()
}

function deleteFrog(id) {
  frogs.value = frogs.value.filter(
    frog => frog.id !== id
  )

  saveFrogs()
}

function updateFrog(updatedFrog) {
  const index = frogs.value.findIndex(
    frog => frog.id === updatedFrog.id
  )

  if (index !== -1) {
    frogs.value[index] = updatedFrog
  }

  saveFrogs()
}

function saveFrogs() {
  localStorage.setItem(
    'frogs',
    JSON.stringify(frogs.value)
  )
}

onMounted(() => {
  const savedFrogs = localStorage.getItem('frogs')

  if (savedFrogs) {
    frogs.value = JSON.parse(savedFrogs)
  }
})
</script>

<template>
  <div class="container">

    <h1>🐸 Frog Keeper</h1>

    <p class="subtitle">
      A completely unnecessary frog collection.
    </p>

    <div class="add-frog">

      <h2>Add a New Frog</h2>

      <input
        v-model="newName"
        type="text"
        placeholder="Frog name"
      />

      <input
        v-model="newColor"
        type="text"
        placeholder="Frog color"
      />

      <input
        v-model="newPlace"
        type="text"
        placeholder="Favorite place"
      />

      <select v-model="newRarity">
        <option>Common</option>
        <option>Uncommon</option>
        <option>Rare</option>
        <option>Legendary</option>
      </select>

      <button @click="addFrog">
        Add Frog 🐸
      </button>

    </div>

    <div class="frog-list">

      <FrogItem
        v-for="frog in frogs"
        :key="frog.id"
        :frog="frog"
        @delete="deleteFrog"
        @update="updateFrog"
      />

    </div>

    <p
      v-if="frogs.length === 0"
      class="empty"
    >
      No frogs yet. The pond is suspiciously empty. 🐸
    </p>

  </div>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #dff7d8;
  font-family: Arial, sans-serif;
}

.container {
  width: 600px;
  max-width: 90%;
  margin: 40px auto;
}

h1 {
  text-align: center;
  color: #28651d;
  font-size: 40px;
}

.subtitle {
  text-align: center;
  color: #555;
  margin-bottom: 30px;
}

.add-frog {
  background: white;
  padding: 20px;
  border-radius: 12px;
  margin-bottom: 25px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

.add-frog h2 {
  color: #347a25;
}

.add-frog input,
.add-frog select {
  width: 100%;
  padding: 11px;
  margin: 6px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.add-frog button {
  width: 100%;
  padding: 11px;
  margin-top: 10px;
  background: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.add-frog button:hover {
  background: #388e3c;
}

.frog-list {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

.empty {
  text-align: center;
  color: #666;
  margin-top: 30px;
}

@media (max-width: 600px) {
  .frog-list {
    grid-template-columns: 1fr;
  }
}
</style>

