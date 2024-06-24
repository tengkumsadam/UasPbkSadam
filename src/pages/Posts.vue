<template>
    <div class="container">
      <div class="header">
        <h1 class="page-title">Artikel Terbaru</h1>
        <div class="filter-section">
          <label for="userSelect">Pilih Penulis:</label>
          <select id="userSelect" v-model="selectedUserId" @change="fetchPosts">
            <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
          </select>
        </div>
      </div>
      <div class="post-list" v-if="posts.length">
        <div v-for="post in posts" :key="post.id" class="post-item">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </div>
      <p v-else class="no-posts">Tidak ada artikel yang tersedia.</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUserId: null,
        posts: [],
      };
    },
    methods: {
      fetchUsers() {
        fetch('https://jsonplaceholder.typicode.com/users')
          .then(response => response.json())
          .then(data => {
            this.users = data;
            if (this.users.length) {
              this.selectedUserId = this.users[0].id;
              this.fetchPosts();
            }
          });
      },
      fetchPosts() {
        if (this.selectedUserId) {
          fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
            .then(response => response.json())
            .then(data => {
              this.posts = data.slice(0, 6); // Limit to 6 posts for display
            });
        }
      },
    },
    mounted() {
      this.fetchUsers();
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 800px;
    margin: 50px auto;
    padding: 30px;
    background: linear-gradient(145deg, #ffc500, #ff6200);
    border-radius: 16px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    font-family: 'Arial', sans-serif;
    color: #333;
  }
  
  .page-title {
    font-size: 36px;
    text-align: center;
    margin-bottom: 20px;
    text-transform: uppercase;
    color: #333;
  }
  
  .header {
    text-align: center;
    margin-bottom: 20px;
  }
  
  .filter-section {
    margin-top: 20px;
  }
  
  label {
    font-size: 18px;
    font-weight: bold;
    color: #333;
    margin-bottom: 10px;
    display: block;
  }
  
  select {
    width: 60%;
    padding: 12px;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    background-color: #fff;
    box-shadow: inset 2px 2px 5px rgba(0, 0, 0, 0.1), inset -2px -2px 5px #ffffff;
    color: #444;
    outline: none;
  }
  
  .post-list {
    margin-top: 20px;
  }
  
  .post-item {
    background-color: #fff;
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    margin-bottom: 20px;
  }
  
  .post-item:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  h3 {
    font-size: 24px;
    color: #333;
    margin-bottom: 10px;
  }
  
  p {
    font-size: 16px;
    color: #666;
    line-height: 1.6;
    padding: 0 20px 20px 20px;
  }
  
  .no-posts {
    text-align: center;
    font-size: 20px;
    color: #333;
    margin-top: 20px;
  }
  </style>
  