<script setup>
import { computed } from 'vue'
let id = 0

const newTodo = ''
const hideCompleted = false
// const todos = ref([
//   { id: id++, text: 'Learn HTML', done: true },
//   { id: id++, text: 'Learn JavaScript', done: true },
//   { id: id++, text: 'Learn Vue', done: false }
// ])
const res = fetch("http://localhost:5153/api/TodoItems", {mode: 'no-cors'}).catch(error=>alert(`Error al cargar: ${error}`))


let todos = res.json

let filteredTodos = todos //computed(() => {
//   return hideCompleted.value
//     ? todos.value.filter((t) => !t.done)
//     : todos.value
// })

function addTodo() {
  let aux = { id: id++, text: newTodo.value, done: false }
  fetch("http://localhost:5153/api/TodoItems", {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(aux),
    mode: 'no-cors'
  }).then((response=>response.json))
    .catch(error=>alert(error))
  
  todos = response
  newTodo.value=""
}

function removeTodo(todo) {
  const res = fetch("http://localhost:5153/api/TodoItems", {
    method: 'DELETE',
    headers: {
      'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(todo.value),
    mode: 'no-cors'
  })
  todos = res.json
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button> 
    </form>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done"> 
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)">X</button> 
        </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
    </button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
