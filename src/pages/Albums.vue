<template>
    <div class="page-container">
      <h1 class="page-title">Daftar Album</h1>
      <div class="album-grid">
        <div v-for="album in albums" :key="album.id" @click="goToAlbum(album.id)" class="album-card">
          <div class="album-thumbnail" :style="{ backgroundImage: 'url(' + album.thumbnailUrl + ')' }">
            <div class="album-overlay">
              <span class="album-title">{{ album.title }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Albums',
    setup() {
      const albums = ref([]);
      const router = useRouter();
  
      const fetchAlbums = async () => {
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/albums');
          albums.value = response.data.map(album => ({
            ...album,
            thumbnailUrl: `https://via.placeholder.com/250/${album.id}`
          }));
        } catch (error) {
          console.error('Error fetching albums:', error);
        }
      };
  
      const goToAlbum = (id) => {
        router.push({ name: 'AlbumPhotos', params: { id } });
      };
  
      onMounted(fetchAlbums);
  
      return {
        albums,
        goToAlbum,
      };
    },
  };
  </script>
  
  <style scoped>
  .page-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 40px;
    background: linear-gradient(145deg, #ff7e5f, #feb47b);
    border-radius: 16px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    font-family: Arial, sans-serif;
    color: #fff;
  }
  
  .page-title {
    font-size: 36px;
    text-align: center;
    margin-bottom: 20px;
  }
  
  .album-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    width: 100%;
    max-width: 1200px;
  }
  
  .album-card {
    position: relative;
    overflow: hidden;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .album-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
  }
  
  .album-thumbnail {
    width: 100%;
    height: 200px;
    background-color: #f0f0f0;
    background-size: cover;
    background-position: center;
    position: relative;
  }
  
  .album-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 10px;
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    text-align: center;
    transition: background-color 0.3s ease;
  }
  
  .album-card:hover .album-overlay {
    background-color: rgba(0, 0, 0, 0.8);
  }
  
  .album-title {
    font-size: 18px;
    font-weight: bold;
  }
  </style>
  