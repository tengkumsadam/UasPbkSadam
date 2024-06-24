<template>
    <div class="todo-container">
      <h1 class="page-title">Kegiatan Harian</h1>
      <div class="todo-list grid gap-4 md:grid-cols-2 lg:grid-cols-3">
        <div v-for="kegiatan in kegiatanList" :key="kegiatan.id" class="todo-item bg-white rounded-lg overflow-hidden shadow-lg hover:shadow-xl">
          <div class="px-6 py-4 flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <input type="checkbox" v-model="kegiatan.isDone" class="form-checkbox h-6 w-6 text-green-500 rounded focus:ring-0" />
              <span :class="{ 'line-through': kegiatan.isDone }" class="text-lg font-semibold text-gray-800">{{ kegiatan.nama }}</span>
            </div>
            <button @click="deleteKegiatan(kegiatan.id)" class="delete-button text-gray-600 hover:text-red-500 focus:outline-none transition-colors duration-300">
              <i class="fas fa-trash-alt"></i>
            </button>
          </div>
          <div :style="{ backgroundColor: kegiatan.backgroundColor }" class="bg-gradient-to-r from-yellow-200 to-yellow-400 px-6 py-3">
            <span class="text-sm text-gray-600">{{ kegiatan.time }}</span>
          </div>
        </div>
      </div>
      <form @submit.prevent="addKegiatan" class="add-form mt-6">
        <div class="flex">
          <input type="text" v-model="newKegiatan.nama" placeholder="Tambah Kegiatan Baru" class="input-text flex-1 p-3 text-lg font-semibold text-gray-800 border border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 transition duration-300" />
          <button type="submit" class="add-button ml-3 px-6 py-3 bg-blue-500 text-white rounded-lg hover:bg-blue-600 focus:outline-none transition duration-300">
            Tambah
          </button>
        </div>
      </form>
      <button @click="showCompletedTasks" class="show-completed-button mt-4 px-6 py-3 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300 focus:outline-none transition duration-300">
        Tampilkan Kegiatan yang Selesai
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        kegiatanList: [
          { id: 1, nama: 'Membaca buku', isDone: false, time: '09:00 AM', backgroundColor: 'rgba(255, 183, 94, 0.8)' },
          { id: 2, nama: 'Olahraga pagi', isDone: false, time: '06:00 AM', backgroundColor: 'rgba(172, 196, 250, 0.8)' },
          { id: 3, nama: 'Menulis artikel', isDone: false, time: '03:00 PM', backgroundColor: 'rgba(248, 113, 113, 0.8)' }
        ],
        newKegiatan: { nama: '' }
      };
    },
    methods: {
      addKegiatan() {
        if (this.newKegiatan.nama.trim()) {
          const colors = ['rgba(255, 183, 94, 0.8)', 'rgba(172, 196, 250, 0.8)', 'rgba(248, 113, 113, 0.8)'];
          const randomColor = colors[Math.floor(Math.random() * colors.length)];
          const currentTime = this.getCurrentTime();
          this.kegiatanList.push({ id: this.kegiatanList.length + 1, nama: this.newKegiatan.nama, isDone: false, time: currentTime, backgroundColor: randomColor });
          this.newKegiatan.nama = '';
        }
      },
      deleteKegiatan(id) {
        this.kegiatanList = this.kegiatanList.filter(kegiatan => kegiatan.id !== id);
      },
      showCompletedTasks() {
        this.kegiatanList = this.kegiatanList.filter(kegiatan => kegiatan.isDone);
      },
      getCurrentTime() {
        const now = new Date();
        let hours = now.getHours();
        const ampm = hours >= 12 ? 'PM' : 'AM';
        hours = hours % 12 || 12;
        const minutes = now.getMinutes().toString().padStart(2, '0');
        return `${hours}:${minutes} ${ampm}`;
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-container {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    font-family: 'Arial', sans-serif;
  }
  
  .page-title {
    text-align: center;
    font-size: 28px;
    color: #333;
    margin-bottom: 20px;
  }
  
  .todo-list {
    margin-top: 20px;
  }
  
  .todo-item {
    transition: transform 0.3s, box-shadow 0.3s;
  }
  
  .todo-item:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  .delete-button {
    transition: color 0.3s;
  }
  
  .delete-button:hover {
    color: #e53e3e;
  }
  
  .input-text {
    transition: border-color 0.3s;
  }
  
  .input-text:focus {
    border-color: #3498db;
  }
  
  .add-button {
    transition: background-color 0.3s;
  }
  
  .add-button:hover {
    background-color: #2c7be5;
  }
  
  .show-completed-button {
    transition: background-color 0.3s;
  }
  
  .show-completed-button:hover {
    background-color: #cbd5e0;
  }
  
  .bg-gradient-to-r {
    background: linear-gradient(to right, #ffffff, #f3f3f3);
  }
  </style>
  