<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';

import LoadingSpinner from '@/components/LoadingSpinner.vue';
import BlurCenterFilter from '@/components/filters/BlurCenterFilter.vue';
import BlurFilter from '@/components/filters/BlurFilter.vue';
import BWFilter from '@/components/filters/BWFilter.vue';
import BW2Filter from '@/components/filters/BW2Filter.vue';
import NoFilter from '@/components/filters/NoFilter.vue';

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
      title: 'Blurred with center',
      filter: BlurCenterFilter,
    },
    {
      title: 'Black and white',
      filter: BWFilter,
    },
    {
      title: 'Black and white 2',
      filter: BW2Filter,
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
  <div class="grid gap-4 max-h-full">
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
.full-image {
  max-height: 75vh;
  @apply h-auto max-w-full mx-auto;
}
.preview-tiles {
  max-height: 25vh;
  @apply flex gap-4 justify-center;

  .preview-tile {
    width: 128px;
    @apply cursor-pointer border rounded;
  }

  .preview-image {
    height: 128px;
    width: 72px;
    margin: 0 auto;
  }
}
</style>
