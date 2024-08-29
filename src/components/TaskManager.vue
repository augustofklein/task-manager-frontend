<template>
    <div class="task-manager">
      <h1>Task Manager</h1>
  
      <div class="create-task">
        <h2>Create Task</h2>
        <input v-model="newTaskDescription" placeholder="Task Description" />
        <button @click="createTask">Create Task</button>
      </div>
  
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
  import axios from 'axios';
  
  export default {

    data() {
      return {
        newTaskDescription: '',
        tasks: []
      };
    },

    created() {
      this.fetchTasks();
    },

    methods: {
        async fetchTasks() {
            try {
            const response = await axios.get('https://localhost:7227/api/v1/Task/all');
            this.tasks = response.data;
            } catch (error) {
            console.error('Error fetching tasks:', error);
            }
        },

        async createTask() {
            try {
                if (!this.newTaskDescription) {
                    console.error('Description is required.');
                    return;
                }
                
                const response = await axios.post(`https://localhost:7227/api/v1/Task/create`, {
                    description: this.newTaskDescription,
                    dateCreated: new Date().toISOString(),
                    dateConcluded: '1900-01-01T00:00:00Z'
                });
                
                this.tasks.push(response.data);
                this.newTaskDescription = '';
            } catch (error) {
                console.error('Error creating task:', error.response?.data || error.message);
            }
        },
        
        async updateTask(task) {
            try {
                if (!task.id || !task.description) {
                    console.error('Task ID and description are required.');
                    return;
                }

                const response = await axios.put(`https://localhost:7227/api/v1/Task/update`, {
                    id: task.id,
                    description: task.description,
                    dateConcluded: task.dateConcluded
                });

                const index = this.tasks.findIndex(t => t.id === task.id);
                if (index !== -1) {
                    this.tasks[index] = response.data;
                }

            } catch (error) {
                console.error('Error updating task:', error.response?.data || error.message);
            }
        },

        async deleteTask(taskId) {
            try {
                await axios.delete(`https://localhost:7227/api/v1/Task/delete/${taskId}`);
                this.tasks = this.tasks.filter(task => task.id !== taskId);
            } catch (error) {
                console.error('Error deleting task:', error);
            }
        }
    }
  };
  </script>