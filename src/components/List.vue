<template>
  <ul role="list" class="divide-y divide-gray-100 bg-white rounded-md p-4 m-4">
    <ListItem v-for="album in sortedAlbums" :key="album.name" :album="album" />
  </ul>
</template>

<script setup>
  import { computed } from 'vue';
  import data from '../data/albums.json';
  import ListItem from './ListItem.vue';
  const albums = data.albums;

  const sortedAlbums = computed(() => {
    return albums.sort((a, b) => new Date(b.year) - new Date(a.year));
  })
</script>