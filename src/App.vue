<template>
  <main>
    <header>
      <img src="./assets/pinia-logo.svg" alt="Pinia Logo">
      <h1>Pinia Tasks</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <nav class="filter">
      <button @click="filter = 'all'">
          All Tasks
      </button>
      <button @click="filter = 'favs'">
          Fav Tasks
      </button>
    </nav>

    <div class="loading" v-if="taskStore.loading">
      Loading Tasks ...
    </div>

    <div class="task-list" v-if="filter === 'all'">

      <p>You have <strong>{{ taskStore.totalCount }}</strong> tasks left to do</p>
      <div v-for="task in taskStore.tasks" :key="task.id">
        <TaskDetails :task="task"/>
      </div>

    </div>

    <div class="task-list" v-if="filter === 'favs'">
      
      <p>You have <strong>{{ taskStore.favCount }}</strong> favs left to do</p>
      <div v-for="task in taskStore.favs" :key="task.id">
        <TaskDetails :task="task"/>
      </div>

    </div>

    <button @click="taskStore.$reset">Reset State</button>
  </main>
</template>

<script setup>
import { useTaskStore } from './stores/TaskStore';
import TaskDetails from './components/TaskDetails.vue';
import TaskForm from './components/TaskForm.vue'
import { onMounted, ref } from 'vue';
import { storeToRefs } from 'pinia';

const taskStore = useTaskStore();

// const { tasks, loading, favs, favCount, totalCount } = storeToRefs(taskStore)

const filter = ref('all');

onMounted(() => {
  taskStore.getTasks()
})



</script>