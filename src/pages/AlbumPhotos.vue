<template>
    <div class="page-container">
      <h1 class="page-title">Galeri Foto</h1>
      <div class="photo-grid">
        <div v-for="photo in photos" :key="photo.id" @click="viewPhoto(photo.url)" class="photo-card">
          <div class="photo-thumbnail" :style="{ backgroundImage: 'url(' + photo.thumbnailUrl + ')' }">
            <div class="photo-overlay">
              <span class="photo-title">{{ photo.title }}</span>
            </div>
          </div>
        </div>
      </div>
      <q-dialog v-model="isPhotoDialogOpen" class="photo-dialog">
        <img :src="currentPhotoUrl" class="full-photo" />
      </q-dialog>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  
  export default {
    name: 'AlbumPhotos',
    props: {
      id: {
        type: Number,
        required: true
      }
    },
    setup(props) {
      const photos = ref([]);
      const isPhotoDialogOpen = ref(false);
      const currentPhotoUrl = ref('');
  
      const fetchPhotos = async () => {
        try {
          const response = await axios.get(`https://jsonplaceholder.typicode.com/photos?albumId=${props.id}`);
          photos.value = response.data.map(photo => ({
            ...photo,
            thumbnailUrl: `https://via.placeholder.com/150/${photo.id}`
          }));
        } catch (error) {
          console.error('Error fetching photos:', error);
        }
      };
  
      const viewPhoto = (url) => {
        currentPhotoUrl.value = url;
        isPhotoDialogOpen.value = true;
      };
  
      onMounted(fetchPhotos);
  
      return {
        photos,
        isPhotoDialogOpen,
        currentPhotoUrl,
        viewPhoto
      };
    }
  };
  </script>
  
  <style scoped>
  .page-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 40px;
    background: linear-gradient(145deg, #63a4ff, #83eaf1);
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
  
  .photo-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 20px;
    width: 100%;
    max-width: 1200px;
  }
  
  .photo-card {
    position: relative;
    overflow: hidden;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .photo-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
  }
  
  .photo-thumbnail {
    width: 100%;
    height: 150px;
    background-color: #f0f0f0;
    background-size: cover;
    background-position: center;
    position: relative;
  }
  
  .photo-overlay {
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
  
  .photo-card:hover .photo-overlay {
    background-color: rgba(0, 0, 0, 0.8);
  }
  
  .photo-title {
    font-size: 14px;
    font-weight: bold;
    margin-top: 5px;
  }
  
  .photo-dialog {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
  }
  
  .full-photo {
    max-width: 100%;
    max-height: 80vh;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  </style>
  