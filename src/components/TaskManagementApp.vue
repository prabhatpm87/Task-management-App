<template>
    <div>
      <h1>Task Management App</h1>
      <h3>Create task</h3>
      <div class="div1">
        <label class="label1">Task Name *</label>
        <input class="input1" type="text" placeholder="Enter Your Task Title" v-model="taskTitle" />
        <br />
        <label class="label=2">Task Description</label>
        <input class="input2" type="text" placeholder="Enter Your Task Description" v-model="taskDescription" />
        <br />
        <button class="buttonCr" @click="handleCreateTask">Create Task</button>
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
      </div>
  
      <!-- Filtering buttons -->
      <div class="2">
        <button class="buttonAl" @click="handleFilterChange('all')">All Tasks</button>
        <button class="buttonCo" @click="handleFilterChange('complete')">Complete Tasks</button>
        <button class="buttonIn" @click="handleFilterChange('incomplete')">Incomplete Tasks</button>
      </div>
  
      <!-- Display the list of tasks -->
      <div class="3">
        <h3>Task List</h3>
        <ul>
          <li v-for="(task, index) in filteredTasks" :key="index">
            <input type="checkbox" :checked="task.completed" @change="() => handleToggleComplete(index)" />
            <strong>{{ task.title }}</strong>: {{ task.description }}
            <button class="buttonDe" @click="() => handleDeleteTask(index)">Delete</button>
          </li>
        </ul>
      </div>
      <div class="3">
        <router-link to="/" class="buttonAl">All Tasks</router-link>
        <router-link to="/complete" class="buttonCo">Complete Tasks</router-link>
        <router-link to="/incomplete" class="buttonIn">Incomplete Tasks</router-link>
      </div>
  
    </div>
  </template>
  
  <script>
  export default {
    props: {
      //filter: String // Receive the filter prop from the router
    },
    data() {
      return {
        tasks: [],
        taskTitle: '',
        taskDescription: '',
        errorMessage: '',
        filters: 'all'
      };
    },
    computed: {
      filteredTasks() {
        if (this.filters === 'all') {
          return this.tasks;
        } else if (this.filters === 'complete') {
          return this.tasks.filter(task => task.completed);
        } else if (this.filters === 'incomplete') {
          return this.tasks.filter(task => !task.completed);
        }
        return this.tasks;
      }
    },
    methods: {
      handleCreateTask() {
        if (!this.taskTitle) {
          this.errorMessage = 'Task Name is required';
          return;
        }
  
        const newTask = {
          title: this.taskTitle,
          description: this.taskDescription,
          completed: false
        };
  
        this.tasks.push(newTask);
  
        this.taskTitle = '';
        this.taskDescription = '';
        this.errorMessage = '';
        this.saveTasksToLocalStorage();
      },
      handleToggleComplete(index) {
        this.tasks[index].completed = !this.tasks[index].completed;
        this.saveTasksToLocalStorage();
      },
      handleDeleteTask(index) {
        this.tasks.splice(index, 1);
      },
      handleFilterChange(newFilter) {
        this.filter = newFilter;
        this.saveTasksToLocalStorage();
      },
      saveTasksToLocalStorage() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
      },
      loadTasksFromLocalStorage() {
        const tasksData = localStorage.getItem('tasks');
        if (tasksData) {
          this.tasks = JSON.parse(tasksData);
        }
      }
    },
    created() {
      this.loadTasksFromLocalStorage();
    },
    watch: {
      tasks: {
        handler() {
          this.saveTasksToLocalStorage();
        },
        deep: true
      }
    }
  };
  </script>
  
  <style>
  * {
    background-color: aqua;
  }
  
  .div1 {
    margin-bottom: 5px;
    width: 200px;
    height: 140px;
  }
  
  .label1 {
    margin-bottom: 10px;
  }
  
  .input1 {
    margin-bottom: 10px;
  }
  
  .input2 {
    margin-bottom: 15px;
    width: 150px;
  }
  
  .buttonCr {
    margin-top: 0px;
    background-color: rgb(21, 103, 196);
  }
  
  .buttonAl {
    background-color: rgb(78, 135, 241);
  }
  
  .buttonCo {
    background-color: rgb(78, 135, 241);
  }
  
  .buttonIn {
    background-color: rgb(78, 135, 241);
  }
  
  .buttonDe {
    background-color: rgb(231, 52, 52);
  }
  </style>
  