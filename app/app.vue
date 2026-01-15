<script setup>
  import {ref} from 'vue'
  
  const newTodo = ref('')// menyimpan teks input
  const todos = ref([])// Menyimpan daftar todo

  //===============ADD TASK FUNCT===============

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

  //==============DELETE FUNC=======================

  function deleteTodo(id){ //function hapus todo berdasar id
    todos.value = todos.value.filter(todo => todo.id !== id) 
  }
  

  //===================EDIT FUNC======================

  function editTask(id){ //function untuk mengedit berdasarkan id
    const todo = todos.value.find(todo => todo.id === id) //ambil editTodo id yang sama dengan id yang dikirim ke fnctiom
    
    if(!todo) return //cek apakah editTodo tidak ada ? jika tdk ketemu funct berhenti

    const newText = prompt("Edit Task :", todo.text) //munculkan input browser dengan judul "Edit Task"

    if(newText !== null && newText.trim() !== "") { //pastikan tidak batal input dan trim() menghapus spasi sisa
      todo.text = newText
    }
  }
</script>

<template>
  <div class="text-center p-5">
    <h1 class="text-2xl">Todo List</h1>

    <div class="mb-3 bg-blue-200">
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
        <button class="mx-20" @click="deleteTodo(todo.id)">Delete</button>
        <button @click="editTask(todo.id)">Edit</button>
      </li>
    </ul>
  </div>
</template>
