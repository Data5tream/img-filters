<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';

import LoadingSpinner from '@/components/LoadingSpinner.vue';
import BlurCenterFilter from '@/components/filters/BlurCenterFilter.vue';
import BlurFilter from '@/components/filters/BlurFilter.vue';
import BWFilter from '@/components/filters/BWFilter.vue';
import BW2Filter from '@/components/filters/BW2Filter.vue';
import NoFilter from '@/components/filters/NoFilter.vue';
import PixelCenterFilter from '@/components/filters/PixelCenterFilter.vue';
import PixelFilter from '@/components/filters/PixelFilter.vue';
import SepiaFilter from '@/components/filters/SepiaFilter.vue';

const props = defineProps<{
  image: string;
}>();

const originalImage = ref<string>(props.image);
const currentPreview = ref<string>(props.image);
const currentFilter = ref<number>(0);
const loading = ref<boolean>(true);

const filterPreviews = ref<Array<{ title: string; filter: object }>>([]);

const updateImages = (val: string) => {
  originalImage.value = val;
  currentPreview.value = val;

  filterPreviews.value = [
    {
      title: 'No filter',
      filter: NoFilter,
    },
    {
      title: 'Blurred',
      filter: BlurFilter,
    },
    {
      title: 'Blurred Center',
      filter: BlurCenterFilter,
    },
    {
      title: 'Pixelated',
      filter: PixelFilter,
    },
    {
      title: 'Pixelated Center',
      filter: PixelCenterFilter,
    },
    {
      title: 'B&W',
      filter: BWFilter,
    },
    {
      title: 'B&W 2',
      filter: BW2Filter,
    },
    {
      title: 'Sepia',
      filter: SepiaFilter,
    },
  ];
};

const changeFilter = (filter: number) => {
  loading.value = true;
  currentFilter.value = filter;
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
  <div class="gallery-container">
    <LoadingSpinner v-if="loading" />
    <div class="full-image" v-if="filterPreviews.length > currentFilter">
      <component :is="filterPreviews[currentFilter].filter" :image="currentPreview" @finished="loading = false" />
    </div>
    <div class="preview-tiles">
      <div v-for="(preview, i) in filterPreviews" :key="preview.title" class="preview-tile" @click="changeFilter(i)">
        <div class="preview-image">
          <component :is="preview.filter" :image="currentPreview" />
        </div>
        <span class="block text-center">{{ preview.title }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped lang="postcss">
.gallery-container {
  display: grid;
  grid-template-rows: 1fr auto;

  @apply gap-4 h-full;
}

.full-image {
  height: 66vh;
  @apply max-w-full mx-auto;
}
.preview-tiles {
  @apply flex py-2 gap-4 overflow-x-scroll;

  .preview-tile {
    flex: 0 0 128px;
    @apply cursor-pointer border rounded;
  }

  .preview-image {
    height: 128px;
    width: 72px;
    margin: 0 auto;
  }
}
</style>
