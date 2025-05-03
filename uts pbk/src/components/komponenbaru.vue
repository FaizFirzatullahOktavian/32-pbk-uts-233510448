<template>
    <div class="container"> //membuat kontainer
      <h1>To Do List</h1>
      <div class="content">
        <div class="add-task">
          <div class="input-wrapper">
            <input v-model="newTask" @keyup.enter="addTask" placeholder="Tambahkan kegiatan..." />
            <button @click="addTask">➕</button>
          </div>
          <div class="filter"> //membuat class untuk menampilkan yang belum selesai saja
            <label>
              <input type="checkbox" v-model="showOnlyUnfinished" />
              Tampilkan hanya yang belum selesai
            </label>
          </div>
        </div>
        <div class="task-list"> // membuat class untuk list 
          <ul>
            <li v-for="(task, index) in filteredTasks" :key="index">
              <input type="checkbox" v-model="task.done" />
              <span :class="{ done: task.done }">{{ task.text }}</span>
              <button @click="removeTask(index)">🗑️</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const tasks = ref([
    { text: 'Mengerjakan tugas kuliah', done: false },
    { text: 'Belanja kebutuhan mingguan', done: true },
    { text: 'Membaca buku VueJS', done: false },
    { text: 'Olahraga pagi', done: true },
    { text: 'Menghubungi teman lama', done: false }
  ]);
  
  const newTask = ref('');
  const showOnlyUnfinished = ref(false);
  
  const addTask = () => {
    if (newTask.value.trim()) {
      tasks.value.push({ text: newTask.value, done: false });
      newTask.value = '';
    }
  };
  
  const removeTask = (index) => {
    tasks.value.splice(index, 1);
  };
  
  const filteredTasks = computed(() =>
    showOnlyUnfinished.value
      ? tasks.value.filter(task => !task.done)
      : tasks.value
  );
  </script>
  
  <style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500&display=swap');
  
  * {
    box-sizing: border-box;
    transition: all 0.3s ease;
  }
  
  body {
    margin: 0;
    padding: 0;
    min-height: 100vh;
    background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
    font-family: 'Orbitron', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #e0e0e0;
  }
  
  .container {
    width: 100%;
    max-width: 800px;
    height: 100vh;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 0;
    padding: 2.5rem 2rem;
    backdrop-filter: blur(12px);
    border: 1px solid rgba(255, 255, 255, 0.15);
    box-shadow: 0 10px 30px rgba(0, 255, 255, 0.1);
    animation: fadeIn 0.8s ease;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  h1 {
    text-align: center;
    font-size: 2rem;
    margin-bottom: 1.5rem;
    color: #00f0ff;
    text-shadow: 0 0 10px rgba(0, 240, 255, 0.7);
  }
  
  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  .add-task {
    margin-bottom: 2rem;
  }
  
  .input-wrapper {
    display: flex;
    gap: 12px;
    justify-content: center;
    align-items: center;
  }
  
  input[type="text"] {
    flex: 1;
    padding: 12px 14px;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(0, 240, 255, 0.3);
    border-radius: 12px;
    color: #fff;
    font-size: 1rem;
    outline: none;
  }
  
  input[type="text"]:focus {
    border-color: #7f5af0;
    box-shadow: 0 0 6px #7f5af0;
  }
  
  button {
    background: linear-gradient(135deg, #00ffff, #7f5af0);
    color: #000;
    padding: 12px 16px;
    border: none;
    border-radius: 12px;
    font-weight: 600;
    cursor: pointer;
    box-shadow: 0 0 8px rgba(0, 255, 255, 0.3);
  }
  
  button:hover {
    transform: scale(1.05);
    box-shadow: 0 0 12px #00ffff;
  }
  
  .filter {
    margin-top: 1rem;
    text-align: center;
    color: #ccc;
    font-size: 0.9rem;
  }
  .filter input {
    transform: scale(1.2);
    margin-right: 6px;
    accent-color: #00ffff;
  }
  
  .task-list {
    width: 100%;
  }
  
  ul {
    list-style: none;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
  }
  
  li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: rgba(255, 255, 255, 0.07);
    padding: 12px 16px;
    border-radius: 12px;
    margin-bottom: 10px;
    border: 1px solid rgba(255, 255, 255, 0.12);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    width: 220px;
    flex-shrink: 0;
  }
  
  li:hover {
    background: rgba(255, 255, 255, 0.1);
    transform: translateX(4px);
  }
  
  li input[type="checkbox"] {
    margin-right: 12px;
    transform: scale(1.2);
    accent-color: #00ffff;
  }
  
  li span {
    flex: 1;
    font-size: 1rem;
    color: #e0e0e0;
  }
  
  li span.done {
    text-decoration: line-through;
    color: #999;
  }
  
  li button {
    background: transparent;
    border: 1px solid #ff5555;
    color: #ff5555;
    padding: 6px 10px;
    border-radius: 8px;
    font-size: 0.9rem;
  }
  
  li button:hover {
    background: #ff5555;
    color: black;
    box-shadow: 0 0 8px #ff5555;
  }
  
  label {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.9rem;
    color: #ccc;
  }
  
  @media (min-width: 768px) {
    .container {
      max-width: 100%;
      padding: 2rem;
    }
  
    .task-list ul {
      justify-content: space-around;
    }
  
    .add-task {
      margin-bottom: 2rem;
    }
  }
  </style>
  