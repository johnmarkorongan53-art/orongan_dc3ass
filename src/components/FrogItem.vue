<script setup>
import { ref } from 'vue'

const props = defineProps({
  frog: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedName = ref(props.frog.name)
const editedColor = ref(props.frog.color)
const editedPlace = ref(props.frog.place)
const editedRarity = ref(props.frog.rarity)

function startEditing() {
  editedName.value = props.frog.name
  editedColor.value = props.frog.color
  editedPlace.value = props.frog.place
  editedRarity.value = props.frog.rarity

  isEditing.value = true
}

function updateFrog() {
  if (
    editedName.value.trim() === '' ||
    editedColor.value.trim() === '' ||
    editedPlace.value.trim() === ''
  ) {
    return
  }

  emit('update', {
    ...props.frog,
    name: editedName.value,
    color: editedColor.value,
    place: editedPlace.value,
    rarity: editedRarity.value
  })

  isEditing.value = false
}

function deleteFrog() {
  emit('delete', props.frog.id)
}
</script>

<template>
  <div class="frog-card">

    <div v-if="!isEditing">

      <div class="frog-icon">
        🐸
      </div>

      <h2>{{ frog.name }}</h2>

      <p>
        <strong>Color:</strong> {{ frog.color }}
      </p>

      <p>
        <strong>Favorite Place:</strong> {{ frog.place }}
      </p>

      <p>
        <strong>Rarity:</strong> {{ frog.rarity }}
      </p>

      <button @click="startEditing">
        Edit
      </button>

      <button class="delete-btn" @click="deleteFrog">
        Delete
      </button>

    </div>

    <div v-else>

      <h2>Edit Frog 🐸</h2>

      <input
        v-model="editedName"
        placeholder="Frog name"
      />

      <input
        v-model="editedColor"
        placeholder="Color"
      />

      <input
        v-model="editedPlace"
        placeholder="Favorite place"
      />

      <select v-model="editedRarity">
        <option>Common</option>
        <option>Uncommon</option>
        <option>Rare</option>
        <option>Legendary</option>
      </select>

      <button @click="updateFrog">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>
</template>

<style scoped>
.frog-card {
  padding: 20px;
  background: #f1ffe9;
  border: 2px solid #76b852;
  border-radius: 12px;
  text-align: center;
}

.frog-icon {
  font-size: 50px;
}

.frog-card h2 {
  color: #347a25;
}

.frog-card p {
  margin: 8px;
}

input,
select {
  display: block;
  width: 90%;
  margin: 8px auto;
  padding: 10px;
  border: 1px solid #aaa;
  border-radius: 5px;
}

button {
  padding: 8px 14px;
  margin: 5px;
  border: none;
  border-radius: 5px;
  background: #4caf50;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #388e3c;
}

.delete-btn {
  background: #e53935;
}

.delete-btn:hover {
  background: #c62828;
}
</style>
