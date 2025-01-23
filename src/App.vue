<template>
  <div id="app">
    <h1>Vue.js To-Do List</h1>

    <div class="todo-input">
      <input type="text" v-model="newTask" placeholder="Enter a new task" @keyup.enter="addTask" />
      <button @click="addTask">Add Task</button>
    </div>

    <ul class="todo-list">
      <li v-for="(task, index) in tasks" :key="index">
        <input
          type="checkbox"
          v-model="task.completed"
          @change="updateTask(index, task.completed)"
        />
        <span :class="{ completed: task.completed }">{{ task.name }}</span>
        <button @click="editTask(index)">Edit</button>
        <button @click="deleteTask(index)">Delete</button>
      </li>
    </ul>

    <div v-if="editIndex !== null" class="edit-modal">
      <h2>Edit Task</h2>
      <input
        type="text"
        v-model="editTaskName"
        placeholder="Edit task name"
        @keyup.enter="saveEdit"
      />
      <button @click="saveEdit">Save</button>
      <button @click="cancelEdit">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      editIndex: null,
      editTaskName: '',
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ name: this.newTask, completed: false })
        this.newTask = ''
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(index) {
      this.editIndex = index
      this.editTaskName = this.tasks[index].name
    },
    saveEdit() {
      if (this.editTaskName.trim() && this.editIndex !== null) {
        this.tasks[this.editIndex].name = this.editTaskName
        this.editIndex = null
        this.editTaskName = ''
      }
    },
    cancelEdit() {
      this.editIndex = null
      this.editTaskName = ''
    },
    updateTask(index, completed) {
      this.tasks[index].completed = completed
    },
  },
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}

.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.edit-modal {
  background: #f9f9f9;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  max-width: 300px;
}
</style>
