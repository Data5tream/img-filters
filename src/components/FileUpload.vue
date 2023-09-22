<script setup lang="ts">
let image: string | null;

const emit = defineEmits<{
  (e: 'upload', value: string): void;
}>();

const selectFile = (ev: Event) => {
  const target = ev.target as HTMLInputElement;
  const file = target.files && (target.files[0] as Blob);

  if (!file) {
    return;
  }

  const reader = new FileReader();
  reader.readAsDataURL(file);
  reader.onload = (e) => {
    emit('upload', e.target?.result as string);
  };
};
</script>

<template>
  <label for="file_input">Upload an image</label>
  <input id="file_input" type="file" @change="selectFile" />
  <img v-if="!!image" :src="image" alt="" />
</template>

<style scoped>
label {
  @apply block mb-2 text-sm font-medium text-gray-900 dark:text-white;
}
input {
  @apply block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 dark:text-gray-400
  focus:outline-none dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400;
}
</style>
