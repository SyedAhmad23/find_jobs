<script setup>
import { onMounted, ref } from "vue";
const name = ref("Syed Ahmad");
const status = ref("pending");
const tasks = ref(["Task 1", "Task 2", "Task 3"]);
const newTask = ref("");
const toggleStatus = () => {
  status.value = status.value === "active" ? "pending" : "active";
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    console.log(data);
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log("Erros fetching Tasks");
  }
});
</script>
<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is In-Active</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <br />
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <br />
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
        <button @click="tasks.splice(index, 1)">Delete</button>
      </span>
    </li>
  </ul>
  <br />
  <button v-on:click="toggleStatus">Change status</button>
</template>
