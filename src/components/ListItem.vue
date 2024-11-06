<template>
      <li class="flex justify-between gap-x-6 py-5 cursor-pointer hover:bg-gray-100 p-4 rounded-md" @click="toggleVisibility">
      <div class="flex min-w-0 gap-x-4">
        <!-- <img class="h-12 w-12 flex-none rounded-full bg-gray-50" :src="album.imageUrl" alt="" /> -->
        <div class="min-w-0 flex-auto">
          <p class="text-sm/6 font-semibold text-gray-900">{{ album.name }}</p>
          <p class="mt-1 truncate text-xs/5 text-gray-500">{{ album.year }}</p>
          <p class="mt-1 truncate text-xs/5 text-gray-500">{{ album.artist }}</p>
          <p v-show="!showDetails" class="mt-1 truncate text-xs/5 text-gray-500">Click to see more...</p>
          <p v-show="showDetails" class="mt-1 truncate text-xs/5 text-gray-500">{{ album.genre }}</p>
        </div>
      </div>
    </li>
</template>

<script setup>
  import { ref } from 'vue';

  defineProps({
    album: {
      name: String,
      year: Number,
      artist: String,
      // imageUrl: String 
    }
  });

  const showDetails = ref(false);
  const toggleVisibility = () => {
    showDetails.value = !showDetails.value;
  }
</script>