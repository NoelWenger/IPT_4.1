<script setup lang="ts">
import { ref, computed } from 'vue'

interface Task {
  text: string
  done: boolean
  high_priority: boolean
  hours: number
}

const tasks = ref<Task[]>([
  { text: 'Rasen mähen',        done: false, high_priority: false, hours: 1 },
  { text: 'Französisch lernen', done: false, high_priority: true,  hours: 2 },
  { text: 'Einkaufen',          done: false, high_priority: true,  hours: 1 },
  { text: 'Abfall rausbringen', done: false, high_priority: true,  hours: 1 },
])

// Eingaben
const newTaskText = ref('')
const newTaskHigh = ref(false)
const newTaskHours = ref<number | null>(null)

// sortierte Tasks: High Priority zuerst
const sortedTasks = computed(() =>
    [...tasks.value].sort((a, b) => (b.high_priority ? 1 : 0) - (a.high_priority ? 1 : 0))
)

// Statuszeile
const doneCount = computed(() => tasks.value.filter(t => t.done).length)
const totalCount = computed(() => tasks.value.length)
const openHours = computed(() =>
    tasks.value.filter(t => !t.done).reduce((sum, t) => sum + t.hours, 0)
)

// Task hinzufügen
function addTask() {
  if (!newTaskText.value || newTaskHours.value === null) return

  tasks.value.push({
    text: newTaskText.value,
    done: false,
    high_priority: newTaskHigh.value,
    hours: newTaskHours.value
  })

  // Eingaben leeren
  newTaskText.value = ''
  newTaskHigh.value = false
  newTaskHours.value = null
}

// Task löschen
function deleteTask(index: number) {
  tasks.value.splice(index, 1)
}
</script>

<template>
  <div class="container">
    <h2>Aufgabenliste</h2>

    <!-- Statuszeile -->
    <p>
      {{ doneCount }} von {{ totalCount }} Tasks sind erledigt,
      {{ openHours }} h Aufwand offen
    </p>

    <!-- Eingabeformular -->
    <div class="form-group">
      <label for="task">Neuer Task</label>
      <input
          class="form-control"
          id="task"
          type="text"
          v-model="newTaskText"
      />
    </div>

    <div class="form-group">
      <label for="hours">Geschätzte Stunden</label>
      <input
          class="form-control"
          id="hours"
          type="number"
          v-model="newTaskHours"
      />
    </div>

    <label for="prio">
      <input id="prio" type="checkbox" v-model="newTaskHigh" />
      Hohe Priorität
    </label>

    <div class="form-actions">
      <button @click="addTask">
        Task hinzufügen
      </button>
    </div>

    <!-- Taskliste -->
    <ul>
      <li
          v-for="(task, index) in sortedTasks"
          :key="index"
          :class="[{ 'is-done': task.done }, { 'high-priority': task.high_priority }]"
      >
        <input type="checkbox" v-model="task.done" />
        [{{ task.hours }}h] {{ task.text }}
        <button @click="deleteTask(index)">Löschen</button>
      </li>
    </ul>
  </div>
</template>

<style>
.is-done {
  text-decoration: line-through;
}

.high-priority {
  font-weight: bold;
  color: red;
}

.form-group {
  display: block;
}

.form-group label {
  display: block;
  margin-bottom: 2px;
}

.form-control {
  width: 100%;
  padding: 2px 5px;
  height: 32px;
  margin-bottom: 5px;
}

.form-actions {
  display: block;
  margin-top: 1rem;
  margin-bottom: 2rem;
}

.container {
  margin: 20px auto;
  max-width: 400px;
  width: 100%;
}
</style>