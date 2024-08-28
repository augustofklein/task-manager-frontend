<template>
  <div class="task-manager">
    <h1>Task Manager</h1>

    <!-- Create New Task -->
    <div class="create-task">
      <h2>Create Task</h2>
      <input v-model="newTaskDescription" placeholder="Task Description" />
      <button @click="createTask">Create Task</button>
    </div>

    <!-- Task List -->
    <div class="task-list">
      <h2>Task List</h2>
      <div v-for="task in tasks" :key="task.id" class="task">
        <p>
          <strong>Description:</strong>
          <input v-model="task.description" />
        </p>
        <p>
          <strong>Created At:</strong> {{ task.dateCreated }}
        </p>
        <p>
          <strong>Date of Conclusion:</strong>
          <input v-model="task.dateConcluded" type="date" />
        </p>
        <button @click="updateTask(task)">Update</button>
        <button @click="deleteTask(task.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTaskDescription: '',
      tasks: [
        // Initial static data. Later, replace this with API data
        {
          id: 1,
          description: 'Sample Task 1',
          dateCreated: '2024-08-28',
          dateConcluded: ''
        },
        {
          id: 2,
          description: 'Sample Task 2',
          dateCreated: '2024-08-27',
          dateConcluded: ''
        }
      ]
    };
  },
  methods: {
    // Create a new task
    createTask() {
      const newTask = {
        id: this.tasks.length + 1,
        description: this.newTaskDescription,
        dateCreated: new Date().toISOString().split('T')[0],
        dateConcluded: ''
      };
      this.tasks.push(newTask);
      this.newTaskDescription = ''; // Clear the input
    },
    
    // Update an existing task
    updateTask(task) {
      // Logic to update the task description and date of conclusion
      const taskIndex = this.tasks.findIndex(t => t.id === task.id);
      if (taskIndex !== -1) {
        this.tasks[taskIndex] = task;
      }
    },

    // Delete a task
    deleteTask(taskId) {
      this.tasks = this.tasks.filter(task => task.id !== taskId);
    }
  }
};
</script>

<style scoped>
.task-manager {
  font-family: Arial, sans-serif;
}

.create-task {
  margin-bottom: 20px;
}

.task-list {
  border-top: 1px solid #ccc;
}

.task {
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

input {
  margin: 5px 0;
  padding: 5px;
}

button {
  margin-right: 10px;
  padding: 5px 10px;
}
</style>