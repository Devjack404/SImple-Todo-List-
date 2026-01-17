<script setup>
  import {ref} from 'vue'
  
  const newTodo = ref('')// menyimpan teks input
  const todos = ref([])// Menyimpan daftar todo

  //ambil data dari daftar todos dan menyimpan ke var savedTodos
  onMounted(() => {
    const savedTodos = localStorage.getItem ('todos') 
    if(savedTodos){
      todos.value = JSON.parse(savedTodos)
    } 
  })

  //simpan data setiap berubah
  watch(todos, (newTodos) => {
    localStorage.setItem('todos', JSON.stringify(newTodos))
  },{ deep : true })


  //===============ADD TASK FUNCT===============

  function addTodo(){
    if(newTodo.value.trim() === '')//manambah nilai ke newTodo dan menghapus spasi
    return 
    todos.value.push({
      id: Date.now(), //membuat id berdasarkan waktu saat ini
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
  <div class="text-center p-5 m-10 border-2 border-blue-500 rounded-lg">
    
    <!-- Judul Halaman -->
    <h1 class="text-4xl font-bold text-blue-500 mb-5">TODO LIST</h1>

    <!-- Form Input Todo untuk bisa menggunakan tombol enter ketika selesai input-->
    <form class="mb-3 bg-blue-200 flex  rounded-lg" @submit.prevent="addTodo">
      <input
      class="w-full h-10 flex px-5" 
      v-model="newTodo"
      type="text"
      placeholder="Masukan Tugas..."
      />
      <button class="px-3 py-2 bg-indigo-400" @click="addTodo">Tambah</button>
    </form>

    <!-- jarak antar kotak todo -->
    <ul class="p-1 space-y-2">
      <li 
        v-for="todo in todos" 
        :key="todo.id"
        class="flex items-center gap-3 bg-blue-100 rounded-lg p-2 space-x-2"
      >
        <!-- Kotak Checkbox -->
        <div>
          <input class="flex items-center w-5 h-5" type="checkbox" v-model="todo.done" />
        </div>
        
          <!-- desain tulisan todo -->
        <span 
          :class="[
            'flex-1 text-left transition-all duration-300 text-lg',
            todo.done
              ? 'line-through text-gray-500'
              : 'no-underline text-black'

          ]"
        >
         {{ todo.text }}
        </span>

        <!-- Tombol Hapus dan Edit -->
        <button class="px-3 py-1 bg-red-500 text-white rounded-lg" @click="deleteTodo(todo.id)">Del</button>
        <button class="px-3 py-1 bg-blue-500 text-white rounded-lg" @click="editTask(todo.id)">Edit</button>
      </li>
    </ul>
  </div>
</template>
