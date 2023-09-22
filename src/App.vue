<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { initFlowbite } from 'flowbite';
import FileUpload from '@/components/FileUpload.vue';
import FilterGallery from '@/components/FilterGallery.vue';

let originalImage = ref('');
let previewImage = ref('');

const onUpload = (img: string) => {
  originalImage.value = img;
  previewImage.value = img;
};

// initialize components based on data attribute selectors
onMounted(() => {
  initFlowbite();
});
</script>

<template>
  <main class="container mx-auto p-4">
    <FileUpload v-if="!originalImage" @upload="onUpload" />
    <FilterGallery v-else :image="originalImage" />
  </main>
</template>
