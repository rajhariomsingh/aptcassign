<template>
  <div id="app" class="container">
    <div class="header">
      <h1>
        AptCoder To-Do List(vuejs)  
        <img src="@/assets/apt.png" alt="To-Do Icon" class="todo-icon" />
      </h1>
      <h4>
      By Hariom Singh,   mail: hariomandrahulsingh@gmail.com
      </h4>
    </div>
    <task-list :tasks="tasks" @completeTask="completeTask"></task-list>
    <div class="add-task">
      <h2>Add Task</h2>
      <div class="input-wrapper">
        <input v-model="newTask" placeholder="New task..." />
        <button @click="addTask">Add</button>
      </div>
    </div>
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskList,
  },
  data() {
    return {
      tasks: [],
      newTask: '',
    };
  },
  methods: {
    async addTask() {
      try {
        const newTask = { title: this.newTask, createdAt: new Date(), completed: false };
        await this.$http.post('/tasks', newTask);
        this.fetchTasks();
        this.newTask = '';
      } catch (error) {
        console.error('Error adding task:', error);
      }
    },
    async completeTask(id) {
      try {
        await this.$http.patch(`/tasks/${id}`, { completed: true, completedAt: new Date() });
        this.fetchTasks();
      } catch (error) {
        console.error('Error completing task:', error);
      }
    },
    async fetchTasks() {
      try {
        const response = await this.$http.get('/tasks');
        this.tasks = response.data;
      } catch (error) {
        console.error('Error fetching tasks:', error);
      }
    },
  },
  mounted() {
    this.fetchTasks();
  },
};
</script>

<style>
.container {
  text-align: center;
}

.header {
  background-color: #333; /* Dark grey header background */
  color: white;
  padding: 10px;
}

h1 {
  display: flex;
  align-items: center;
  justify-content: center;
}

.todo-icon {
  margin-left: 10px;
  width: 24px;
  height: 24px;
}

.add-task {
  margin-top: 20px;
}

.input-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 60%;
  margin: auto;
}

input {
  width: 70%;
  padding: 10px;
  border: 1px solid #666;
  border-radius: 5px 0 0 5px;
}

button {
  width: 30%;
  padding: 10px;
  border: 1px solid #666;
  border-radius: 0 5px 5px 0;
  background-color: #333;
  color: white;
  cursor: pointer;
}
.todo-icon {
  width: 50px; /* Adjust the size as needed */
  height: 50px;
}
</style>
