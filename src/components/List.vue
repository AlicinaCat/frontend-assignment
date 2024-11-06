<template>
  <ul role="list" class="divide-y divide-gray-100 bg-white rounded-md p-4 mx-auto my-4 md:w-3/5 lg:w-3/5">
      <div class="mt-10">
        <div class="mt-2">
          <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600">
            <input v-model="searchFilter" type="text" name="searchFilter" id="search" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 flex  select-none items-center pl-3 text-gray-500" placeholder="Search..." />
          </div>
        </div>
      </div>
    <ListItem v-for="album in filteredAlbums" :key="album.name" :album="album" />
  </ul>
</template>

<script setup>
  import { computed, ref } from 'vue';
  import data from '../data/albums.json';
  import ListItem from './ListItem.vue';
  const albums = data.albums;

  const searchFilter = ref('');

  const sortedAlbums = albums.sort((a, b) => new Date(b.year) - new Date(a.year));

  const filteredAlbums = computed(() => {
    return searchFilter.value === '' ? 
                    sortedAlbums : 
                    sortedAlbums.filter(album => album.name.toLowerCase().includes(searchFilter.value));

  })
</script>