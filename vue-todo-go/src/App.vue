<script setup>
import { ref, reactive, onMounted } from "vue";

const state = reactive({
  todos: [],
});

const newTask = ref("");
let fakeId = 4;

const getTodos = () => {
  try {
    fetch("/api/todos")
      .then((response) => response.json())
      .then((data) => (state.todos = data));
  } catch (error) {
    console.log("Could not fetch todos", error);
  }
};

const handleSubmit = async () => {
  const task = {
    id: fakeId.toString(),
    item: newTask.value,
    completed: false,
  };
  fakeId++;
  fetch("/api/todos", { method: "POST", body: JSON.stringify(task) })
    .then((newTask.value = ""))
    .then(getTodos());
};

onMounted(() => {
  getTodos();
});
</script>

<template>
  <h1>TODO List</h1>
  <form @submit.prevent="handleSubmit">
    <label for="newTask">Add TODO</label>
    <input type="text" name="newTask" v-model="newTask" />
    <input type="submit" value="Submit" />
  </form>
  <ul>
    <li v-for="todo in state.todos" :key="todo.id">{{ todo.item }}</li>
  </ul>
</template>
