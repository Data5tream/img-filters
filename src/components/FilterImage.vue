<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Konva from 'konva';

const props = defineProps<{
  image: string;
}>();

const stage = ref();
const layer = ref();

const loadFilter = (dataUrl: string) => {
  Konva.Image.fromURL(dataUrl, function (imgNode) {
    const lay = layer.value.getNode();
    const mainScale = lay.getHeight() / imgNode.getHeight();

    imgNode.setAttrs({
      x: 0,
      y: 0,
      scaleX: mainScale,
      scaleY: mainScale,
    });
    lay.add(imgNode);

    applyFilter();

    Konva.Image.fromURL(dataUrl, function (imgNode) {
      const miniScale = 0.4;

      imgNode.setAttrs({
        x: lay.getWidth() / 2 - imgNode.getWidth() * miniScale * mainScale,
        y: lay.getHeight() / 2 - imgNode.getHeight() * miniScale * mainScale,
        scaleX: miniScale,
        scaleY: miniScale,
      });
      lay.add(imgNode);
    });
  });
};

const applyFilter = () => {
  const konvaImg = layer.value.getNode().findOne('Image');
  konvaImg.cache();
  konvaImg.filters([Konva.Filters.Blur]);
  konvaImg.blurRadius(20);
};

onMounted(() => {
  loadFilter(props.image);
});
</script>

<template>
  <v-stage ref="stage" :config="{ width: 800, height: 800 }">
    <v-layer ref="layer" />
  </v-stage>
</template>
