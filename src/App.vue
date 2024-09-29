<template>
  <div>
    <h1>TodoList</h1>
    <form id="todo-form" @submit="handleAdd">
      <input
        v-model="valueInput"
        type="text"
        id="todo-input"
        placeholder="Nhập công việc mới..."
        required
      />
      <button type="submit">Thêm</button>
    </form>
    <ul id="todo-list">
      <li v-for="job in listJobs" :key="job.id">
        <input @click="handleClick(job.id)" type="checkbox" v-model="job.status">
        <h4 :style="{ textDecoration: job.status ? 'line-through' : 'none' }">{{ job.name }}</h4>
        <button @click="handleDelete(job.id)" class="delete-btn">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const valueInput = ref("");

const data = [
  { id: 1, name: "Code", status: false },
  { id: 2, name: "Review code", status: true },
  { id: 3, name: "Test application", status: false },
  { id: 4, name: "Fix bugs", status: true },
  { id: 5, name: "Deploy project", status: false },
];

if (!localStorage.getItem("data")) {
  localStorage.setItem("data", JSON.stringify(data));
}

const listJobs = ref(JSON.parse(localStorage.getItem("data")));

const handleDelete = (id) => {
  listJobs.value = listJobs.value.filter((job) => job.id !== id);
}

const handleAdd = (event) => {
  event.preventDefault();
  if (valueInput.value.trim() === "") return;
  
  const newJob = {
    id: listJobs.value.length ? listJobs.value[listJobs.value.length - 1].id + 1 : 1,
    name: valueInput.value,
    status: false,
  }
  listJobs.value.push(newJob);
  valueInput.value = "";
}

const handleClick = (id) => {
  const job = listJobs.value.find((job) => job.id === id);
  if (job) {
    job.status = !job.status;
    localStorage.setItem("data", JSON.stringify(listJobs.value));
  }
}


</script>

<style>
body {
  font-family: Arial, sans-serif;
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}
h1 {
  text-align: center;
}
#todo-form {
  display: flex;
  margin-bottom: 20px;
}
#todo-input {
  flex-grow: 1;
  padding: 5px;
  font-size: 16px;
}
button {
  padding: 5px 10px;
  font-size: 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
}
.delete-btn {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  background-color: #f9f9f9;
  padding: 10px;
  margin-bottom: 5px;
  display: flex;
  align-items: center;
}
li h4 {
  flex-grow: 1;
  margin: 0 10px;
}
</style>
