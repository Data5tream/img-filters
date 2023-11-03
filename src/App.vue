<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { initFlowbite } from 'flowbite';
import FileUpload from '@/components/FileUpload.vue';
import FilterGallery from '@/components/FilterGallery.vue';

const originalImage = ref<string>();
const previewImage = ref<string>();

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
  <main>
    <FileUpload v-if="!originalImage" @upload="onUpload" />
    <FilterGallery v-else :image="originalImage" />
  </main>
</template>

<style lang="postcss">
main {
  height: 100vh;
  @apply p-4;
}
</style>
