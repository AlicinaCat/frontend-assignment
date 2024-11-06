<template>
  <ul role="list" class="divide-y divide-gray-100 bg-white rounded-md p-4 m-4">
    <ListItem v-for="album in albums" :key="album.name" :album="album" />
  </ul>
</template>

<script setup>
  import data from '../data/albums.json';
  import ListItem from './ListItem.vue';
  const albums = data.albums;
</script>