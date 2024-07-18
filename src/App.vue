<template>
  <h1>ToDo App</h1>
  <form @submit.prevent="addTodo()">
    <label>New ToDo </label>
    <input
        v-model="newTodo"
        name="newTodo"
        autocomplete="off"
    >
    <button>Add ToDo</button>
  </form>
  <h2>ToDo List</h2>
  <ul>
    <li
        v-for="(todo, index) in todos"
        :key="index"
    >
			<span
          :class="{ done: todo.done }"
          @click="doneTodo(todo)"
      >{{ todo.fullName }}</span>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script setup>
import {inject, onMounted, ref} from 'vue';

const axios = inject('axios')  // inject axios

const newTodo = ref('');

const defaultData = [{
  done: false,
  fullName: 'Write a blog post'
}]

const todos = ref(defaultData);

function addTodo() {
  if (newTodo.value) {
    todos.value.push({
      done: false,
      content: newTodo.value
    });
    newTodo.value = '';
  }
  axios.post('/api/todos', {
    title: newTodo.value,
    description: 'This is a description'
  }).then((response) => {
    console.log(response.data)
  })
}

function doneTodo(todo) {
  todo.done = !todo.done
  axios.put('/api/todos', {
    title: newTodo.value,
    description: 'This is a description'
  }).then((response) => {
    console.log(response.data)
  })
}

function removeTodo(index) {
  todos.value.splice(index, 1);
}

onMounted(() => {
  axios.get('/api/commercials').then((response) => {
    todos.value = response.data
  })
})
</script>
