<template>
    <q-page class="container">
      <h1 class="title">Home Page</h1>
      <AlbumPhotos />
    </q-page>
  </template>
  
  <script>
  import AlbumPhotos from './AlbumPhotos.vue';
  
  export default {
    name: 'HomePage',
    components: {
      AlbumPhotos,
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 900px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f90000;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .title {
    text-align: center;
    font-size: 28px;
    color: #333;
    margin-bottom: 20px;
  }
  </style>
  