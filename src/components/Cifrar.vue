<template>
  <div class="flex justify-center text-center">
    <h1 class="text-3xl my-5 ">Ingreso de Imagenes</h1>
  </div>


  <div class="flex justify-center text-center">
    <div class="grid grid-cols-2">
      <input class="file-input w-full max-w-xs my-5" type="file" accept="image/*" @change="handleImageChange(1)">
      <img class="h-96 w-96 my-5" v-if="img1Src" :src="img1Src" alt="Image 1">

      <!-- Input for selecting the second image -->
      <input class="file-input w-full max-w-xs my-5" type="file" accept="image/*" @change="handleImageChange(2)">
      <img class="h-96 w-96" v-if="img2Src" :src="img2Src" alt="Image 2">

      <!-- Button to apply the filter -->

    </div>
    <!-- Input for selecting the first image -->

  </div>
  <div class="flex justify-center text-center">
    <div class="grid">
      <button class="btn btn-neutral mb-2" @click="applyFilter">Aplicar Cifrado</button>
      <img class="h-96 w-96" v-if="filteredImgSrc" :src="filteredImgSrc" alt="Filtered Image">
    </div>

  </div>


  <!-- Display the filtered image -->
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
          },
          responseType: 'blob'
        });
        console.log(response.data);



        const imagenfilt = URL.createObjectURL(response.data);
        console.log(imagenfilt);
        this.filteredImgSrc = imagenfilt

      } catch (error) {
        console.error('Error applying filter:', error);
      }
    }
  },
};
</script>
