<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';
import Konva from 'konva';

const props = defineProps<{
  image: string;
}>();

const emit = defineEmits<{
  (e: 'finished'): void;
}>();

const container = ref<HTMLDivElement>();
const layer = ref();

const width = computed(() => 1080 * baseScale.value);
const height = computed(() => 1920 * baseScale.value);

const baseScale = ref(1);

const loadFilter = (dataUrl: string) => {
  // Create background image
  Konva.Image.fromURL(dataUrl, function (imgNode) {
    const lay = layer.value.getNode() as Konva.Layer;

    // Get the scale that fits the image onto the canvas
    const mainScale = height.value / imgNode.getHeight();

    // Center image horizontally onto the canvas
    imgNode.setAttrs({
      x: (width.value - imgNode.getWidth() * mainScale) / 2,
      y: 0,
      scaleX: mainScale,
      scaleY: mainScale,
    });
    imgNode.cache({ pixelRatio: 1 });
    lay.add(imgNode);

    // Blur the first image
    applyFilter();

    // Emit finished event in case this filter image is used to trigger the loading indicator
    emit('finished');
  });
};

const applyFilter = () => {
  const konvaImg = layer.value.getNode().findOne('Image');
  konvaImg.filters([Konva.Filters.Grayscale, Konva.Filters.Contrast, Konva.Filters.Brighten]);
  konvaImg.brightness(0.2);
  konvaImg.contrast(0.8);
};

onMounted(() => {
  loadFilter(props.image);

  baseScale.value = container.value!.clientHeight / 1920;
});
</script>

<template>
  <div class="canvas-container" ref="container">
    <v-stage :config="{ width, height }">
      <v-layer ref="layer" />
    </v-stage>
  </div>
</template>

<style scoped>
.canvas-container {
  height: 100%;
}
</style>
