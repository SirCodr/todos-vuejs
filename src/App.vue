<script setup lang="ts">
import { ref } from 'vue'

type Task = {
  id: number
  title: string
  completed: boolean
}

const text = ref('')
const tasks = ref<Task[]>([])

function addTask(title: string) {
  tasks.value.push({
    id: tasks.value.length + 1,
    title,
    completed: false
  })
  text.value = ''
}

function removeTask(id: number) {
  const taskIndexFound = tasks.value.findIndex(task => task.id === id)

  if (taskIndexFound === -1) return

  tasks.value.splice(taskIndexFound, 1)
}

</script>

<template>
  <main>
    <div>
      <section>
        <h2>
          Task Tracker
        </h2>
        <form @submit.prevent="addTask(text)" class="input-group">
          <input type="text" placeholder="Start writing and press enter to create task" v-model="text" />
          <button>
            Submit
          </button>
        </form>
      </section>
      <section>
        <ul class="task-list">
          <li v-for="task in tasks" class="task-item" :class="{ completed: task.completed }">
            <div>
              <input type="checkbox" name="completed" :value="task.completed"
                @change="e => task.completed = !task.completed">
              <span>{{ task.title }}</span>
            </div>
            <button @click="removeTask(task.id)">
              Delete
            </button>
          </li>
        </ul>
      </section>
    </div>
  </main>
</template>

<style scoped>
main {
  display: flex;
  justify-content: center;
  width: 100%;

  > div {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 35px;
    width: 300px;
  }
}

input[type="text"] {
  border-radius: 5px;
  padding: 3px 4px;
}

button {
  border-radius: 5px;
}

section {
  width: 100%;
}

.input-group {
  display: flex;
  gap: 7px;

  input {
    flex: 1;
  }
}

.task-list {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.task-item {
  display: flex;
  justify-content: space-between;

  > div {
    display: flex;
    gap: 7px;
    flex: 1;
  }
}

.task-item.completed {
  span {
    text-decoration: line-through;
  }
}
</style>
