<template>
    <div>
      <h1>Task Management App</h1>
  
     
      <button class="button mt-4" @click="toggleForm">{{ showForm ? 'Hide Add Task Form' : 'Show Add Task Form' }}</button>
  
    
      <div v-if="showForm" >
        <input class="mt-8 mb-8" v-model="newTask" placeholder="Enter task name" />
        <button class="button mt-4" @click="addTask">Add Task</button>
      </div>
  
      
      <ul v-if="tasks.length > 0">
        <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }">
            {{ task.name }}
            <button class="button mt-4" @click="toggleComplete(task)">{{ task.completed ? 'UNDO' : 'COMPLETE' }}</button>
            <button class="button mt-4" @click="deleteTask(index)">DELETE</button>
        </li>
      </ul>
      <p v-else>No tasks available</p>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';

    export default {
    setup() {
        
        const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || []); // Persist tasks in localStorage
        const newTask = ref(''); 
        const showForm = ref(false); 

        
        const toggleForm = () => {
        showForm.value = !showForm.value;
        };

        
        const addTask = () => {
        if (newTask.value.length >= 3) {
            
            tasks.value.push({ name: newTask.value, completed: false });

            
            console.log("Task added:", tasks.value);

            
            newTask.value = '';

            // Save to localStorage
            localStorage.setItem('tasks', JSON.stringify(tasks.value));
        } else {
            alert('Task name must be at least 3 characters long');
        }
        };

        // Function to delete a task
        const deleteTask = (index) => {
        tasks.value.splice(index, 1); // Remove task
        localStorage.setItem('tasks', JSON.stringify(tasks.value)); // Update localStorage
        };

        // Function to mark a task as complete/incomplete
        const toggleComplete = (task) => {
        task.completed = !task.completed;
        localStorage.setItem('tasks', JSON.stringify(tasks.value)); // Update localStorage
        };

        return {
        tasks,
        newTask,
        showForm,
        toggleForm,
        addTask,
        deleteTask,
        toggleComplete,
        };
    },

    
    };




  </script>
  
  <style scoped>
    .completed {
        background-color: lightgreen;
        border: 2px solid red;
    }

  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    margin: 10px 0;
    padding: 10px;
    background-color: #f0f0f0;
  }
  button {
    margin-left: 10px;
  }
  </style>
  