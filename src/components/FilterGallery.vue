<script setup lang="ts">
import type { Ref } from 'vue';
import { onMounted, ref, watch } from 'vue';

import FilterImage from '@/components/FilterImage.vue';

const props = defineProps<{
  image: string;
}>();

const originalImage: Ref<string> = ref(props.image);
const currentPreview: Ref<string> = ref(props.image);

const filterPreviews: Ref<Array<{ title: string; img: string }>> = ref([]);

const updateImages = (val: string) => {
  originalImage.value = val;
  currentPreview.value = val;

  let filters = [];
  for (let i = 0; i < 5; i++) {
    filters.push({
      title: `Filter ${i}`,
      img: val,
    });
  }
  filterPreviews.value = filters;
};

watch(
  () => props.image,
  (val: string) => updateImages(val)
);

onMounted(() => {
  updateImages(props.image);
});
</script>

<template>
  <div class="grid gap-4 max-h-full">
    <div>
      <FilterImage :image="currentPreview" />
    </div>
    <div class="grid grid-cols-5 gap-4">
      <div v-for="preview in filterPreviews" :key="preview.title">
        <img class="h-auto max-w-full rounded-lg" :src="preview.img" :alt="preview.title" />
        <span class="block text-center">{{ preview.title }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped lang="postcss">
.preview-image {
  max-height: 75vh;
  @apply h-auto max-w-full rounded-lg mx-auto;
}
</style>
