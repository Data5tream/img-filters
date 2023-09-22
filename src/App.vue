<script setup lang='ts'>
import { onMounted } from 'vue';
import { initFlowbite } from 'flowbite';

let file: File;

// initialize components based on data attribute selectors
onMounted(() => {
  initFlowbite();
})
</script>

<template>
  <div>
    <h1>Test</h1>
  </div>
</template>
