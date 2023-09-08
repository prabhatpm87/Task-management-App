<template>
  <div>
    <h1 class="h1">Task Management App</h1>
    <h2>Create task</h2>
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

    <div class="2">
      <button class="buttonAl" @click="handleFilterChange('all')">All Tasks</button>
      <button class="buttonCo" @click="handleFilterChange('complete')">Complete Tasks</button>
      <button class="buttonIn" @click="handleFilterChange('incomplete')">Incomplete Tasks</button>
    </div>

    <!-- <div class="navigation-links">
      <router-link to="/all" class="buttonAl">All Tasks</router-link>
      <router-link to="/complete" class="buttonCo">Complete Tasks</router-link>
      <router-link to="/incomplete" class="buttonIn">Incomplete Tasks</router-link>
    </div> -->
    <div class="3">
    <h3>Task List</h3>
    <ul>
      <label class="label=TaskComplete">Task Competed</label>
      <br />
      <li v-for="(task, index) in filteredTasks" :key="index">
        <input class="inputCh" type="checkbox" :checked="task.completed" @change="() => handleToggleComplete(index)" />
        <strong>{{ task.title }}</strong>:
        <template v-if="!task.editing">
          {{ task.description }}
          <button class="buttonEdit" @click="handleEditTask(index)">Edit</button>
          <button class="buttonDe" @click="() => handleDeleteTask(index)">Delete</button>
        </template>
        <template v-else>
          <input type="text" v-model="task.editedDescription" />
          <button class="btoutnSave" @click="handleSaveTask(index)">Save</button>
          <button class="buttonCancel" @click="handleCancelEdit(index)">Cancel</button>
        </template>
      </li>
    </ul>
  </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      taskTitle: '',
      taskDescription: '',
      errorMessage: '',
      filter: 'all'
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'all') {
        return this.tasks;
      } else if (this.filter === 'complete') {
        return this.tasks.filter(task => task.completed);
      } else if (this.filter === 'incomplete') {
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
    },
    handleEditTask(index) {
      this.tasks[index].editing = true;
      this.tasks[index].editedDescription = this.tasks[index].description;
    },
    handleSaveTask(index) {
      if (this.tasks[index].editedDescription.trim() !== "") {
        this.tasks[index].description = this.tasks[index].editedDescription;
        this.tasks[index].editing = false;
        this.saveTasksToLocalStorage();
      }
    },
    handleCancelEdit(index) {
      this.tasks[index].editing = false;
    },
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
  width: 900px;
  height: 100px;
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
  background-color: rgb(54, 228, 54);
  margin-right: 5px;
}

.buttonCo {
  background-color: rgb(250, 235, 27);
  margin-right: 5px;
}

.buttonIn {
  background-color: rgb(230, 130, 16);
}

.buttonDe {
  background-color: rgb(231, 52, 52);
}

.inputCh {
  margin-right: 100px;
}

.h1 {
  text-align: center;
  color: blue;
}

.div2 {
  margin-top: 150px;
}

.error {
  margin-left: 350px;
  margin-bottom: 180px;
  color: rgb(211, 0, 0);
  font-weight: bold;
}
.buttonEdit {
    background-color: rgb(172, 219, 42);
  }
.btoutnSave {
  background-color: rgb(6, 231, 6);
}
.buttonCancel {
  background-color: rgb(177, 64, 64);
}
</style>
