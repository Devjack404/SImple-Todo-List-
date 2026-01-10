<script setup>
  import {ref} from 'vue'
  
  const newTodo = ref('')// menyimpan teks input
  const todos = ref([])// Menyimpan daftar todo

  function addTodo(){
    if(newTodo.value.trim() === '')//manambah nilai ke newTodo dan menghapus spasi
    return 
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      done: false
    })
    newTodo.value = ''
  }

  function deleteTodo(id){ //function hapus todo berdasar id
    todos.value = todos.value.filter(todo => todo.id !== id) 
  }
</script>

<template>
  <div class="container">
    <h1>Todo List</h1>

    <div class="input-group">
      <input 
      v-model="newTodo"
      type="text"
      placeholder="Masukan Tugas..."
      />
      <button @click="addTodo">Tambah</button>
    </div>
    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{done: todo.done}">
         {{ todo.text }}
        </span>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .container{
    background-color : blue;
    max-width : 400px;
    margin : 50px auto;
  }
</style>