<template>
  <h1>ToDo App</h1>
  <form @submit.prevent="addTodo()">
    <div>
      <label>Titre</label>
      <input
          v-model="newTodo.title"
          name="newTodo"
          autocomplete="off"
      >
    </div>
    <div>
      <label>Description </label>
      <input
          v-model="newTodo.description"
          name="newTodo"
          autocomplete="off"
      >
    </div>
    <div>
      <button>Add ToDo</button>
    </div>
  </form>
  <h2>ToDo List</h2>
  <ul>
    <li
        v-for="(todo, index) in todos"
        :key="index"
    >
      <h3>{{todo.title}}</h3>
      <p>{{todo.description}}</p>
      <p>Crée le {{todo.createdAt}}</p>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script setup>
import {inject, onMounted, ref} from 'vue';

const axios = inject('axios')  // inject axios

const blankTodo = {title: '', description: ''};

const newTodo = ref({...blankTodo});

const defaultData = [{
  done: false,
  title: 'Write a blog post',
  description: 'This is a description'
}]

const todos = ref(defaultData);

function addTodo() {
  axios.post('/api/todos', newTodo).then(() => {
    fetchData()
    newTodo.value = {...blankTodo}
  })
}

function removeTodo(index) {
  todos.value.splice(index, 1);

  axios.delete(`/api/todos${index}`).then(() => {
    fetchData()
  })
}

const fetchData = () => {
  axios.get('/api/todos').then((response) => {
    todos.value = response.data
  })
};

onMounted(() => {
  fetchData()
})
</script>
