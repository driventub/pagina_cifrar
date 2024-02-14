<template>
  <div>
    <!-- Input for selecting the first image -->
    <input type="file" accept="image/*" @change="handleImageChange(1)">
    <img v-if="img1Src" :src="img1Src" alt="Image 1">

    <!-- Input for selecting the second image -->
    <input type="file" accept="image/*" @change="handleImageChange(2)">
    <img v-if="img2Src" :src="img2Src" alt="Image 2">

    <!-- Button to apply the filter -->
    <button @click="applyFilter">Apply Filter</button>

    <!-- Display the filtered image -->
    <img v-if="filteredImgSrc" :src="filteredImgSrc" alt="Filtered Image">
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      img1: null,
      img2: null,
      img1Src: null,
      img2Src: null,
      filteredImgSrc: null,
    };
  },
  methods: {
    handleImageChange(imageNumber) {
      // Get the selected file
      const file = event.target.files[0];
      // Update the reactive property accordingly
      if (imageNumber === 1) {
        this.img1 = file;
        this.img1Src = URL.createObjectURL(file);
      } else if (imageNumber === 2) {
        this.img2 = file;
        this.img2Src = URL.createObjectURL(file);
      }
    },
    async applyFilter() {
      const formData = new FormData();
      formData.append('img1', this.img1);
      formData.append('img2', this.img2);
      
      try {
        const response = await axios.post('http://127.0.0.1:8000/uploadfile/', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });

        const blob = new Blob([response.data], { type: 'image/jpeg' });
        this.filteredImgSrc = URL.createObjectURL(blob);
      } catch (error) {
        console.error('Error applying filter:', error);
      }
    }
  },
};
</script>
