<script setup>
import { ref, computed, onMounted } from 'vue'

let newTodo = ref('')
const hideCompleted = ref(false)

let todos = ref([])
onMounted(async () => {
    let res = await fetch("http://localhost:5153/api/TodoItems").catch(error=>alert(`Error al cargar: ${error}`))
    let data = await res.json()
    todos.value = data
})

 let filteredTodos = computed(() => {
    return hideCompleted.value
      ? todos.value.filter((t) => !t.isComplete)
      : todos.value
 })

async function addTodo() {
  let aux = { name: newTodo.value, isComplete: false }
  let json={
    method: 'POST',
    headers: {
      'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(aux)
  }

  let response = await fetch("http://localhost:5153/api/TodoItems", json).catch(error=>alert(error))
  let data = await response.json()
  todos.value.push(data)
  newTodo.value=''
}

async function removeTodo(id) {
  const res = await fetch(`http://localhost:5153/api/TodoItems/${id}`, {
    method: 'DELETE',
  })
  if(res.status==204 || res.status==200){
    //eliminar todo por id
    todos.value = todos.value.filter(t=>t.id != id)
  } else{
    alert("Error al eliminar todo")
  }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button> 
    </form>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.isComplete"> 
        <span :class="{ done: todo.isComplete }">{{ todo.name }}</span> 
        <button @click="removeTodo(todo.id)">X</button> 
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
