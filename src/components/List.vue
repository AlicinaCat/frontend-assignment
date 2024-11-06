<template>
  <div id="list-container" class="bg-white rounded-md p-4 mx-auto my-4 md:w-3/5 lg:w-3/5">
    <!-- search input -->
    <div class="mt-10">
      <div class="mt-2">
        <div
          class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600"
        >
          <input
            v-model="searchFilter"
            type="text"
            name="searchFilter"
            id="search"
            class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 flex select-none items-center pl-3 text-gray-500"
            placeholder="Search..."
          />
        </div>
      </div>
    </div>

    <!-- sorting filters -->
    <div class="my-4">
      <a
        href="#"
        class="text-gray-900 my-2 rounded-md shadow-sm relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
        @click="sortBy('year')"
        >Sort by year</a
      >
      |
      <a
        href="#"
        class="text-gray-900 my-2 rounded-md shadow-sm relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
        @click="sortBy('name')"
        >Sort by title</a
      >
      |
      <a
        href="#"
        class="text-gray-900 my-2 rounded-md shadow-sm relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
        @click="sortBy('artist')"
        >Sort by artist</a
      >
      |
      <a
        href="#"
        class="text-gray-900 my-2 rounded-md shadow-sm relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0"
        @click="sortBy('genre')"
        >Sort by genre</a
      >
    </div>

    <!-- albums list -->
    <ul role="list" class="divide-y divide-gray-100">
      <ListItem
        v-for="(album, index) in displayedAlbums"
        :key="index"
        :album="album"
      />
    </ul>

    <!-- pagination -->
    <div class="flex justify-center m-2">
      <button
        :class="[
          { 'bg-gray-200 text-white': isActivePage(page) },
          'relative inline-flex items-center px-4 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0',
        ]"
        v-for="page in pages"
        :key="page"
        @click="changePage(page)"
      >
        {{ page }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch } from "vue";
import data from "../data/albums.json";
import ListItem from "./ListItem.vue";

const albums = data.albums;
const albumsPerPage = 10;

const currentPage = ref(1);
const searchFilter = ref("");
const sortMethod = ref("year");

const displayedAlbums = computed(() => {
  const startIndex = (currentPage.value - 1) * albumsPerPage;
  const endIndex = startIndex + albumsPerPage;

  return filteredAlbums.value.slice(startIndex, endIndex);
});

const filteredAlbums = computed(() => {
  return searchFilter.value === ""
    ? sortedAlbums.value
    : sortedAlbums.value.filter((album) =>
        album.name.toLowerCase().includes(searchFilter.value)
      );
});

const pageCount = computed(() => {
  return Math.ceil(filteredAlbums.value.length / albumsPerPage);
});

const pages = computed(() => {
  return Array.from({ length: pageCount.value }, (_, i) => i + 1);
});

const sortedAlbums = computed(() => {
  return [...albums].sort((a, b) => {
    switch (sortMethod.value) {
      case "artist":
      case "genre":
      case "name":
        const nameA = a[sortMethod.value]?.toLowerCase();
        const nameB = b[sortMethod.value]?.toLowerCase();
        if (nameA < nameB) return -1;
        if (nameA > nameB) return 1;
        return 0;
      case "year":
      default:
        return new Date(b.year) - new Date(a.year);
    }
  });
});

const changePage = (pageNumber) => {
  if (pageNumber >= 1 && pageNumber <= pageCount.value) {
    currentPage.value = pageNumber;
  }
};

const isActivePage = (page) => {
  return page === currentPage.value;
};

const sortBy = (method) => {
  sortMethod.value = method;
  currentPage.value = 1;
};

watch(currentPage, async () => {
  const scrollTopElement = document.getElementById('list-container');
  if (scrollTopElement) {
    scrollTopElement.scrollIntoView({
      behavior: 'smooth',
      block: 'start',
    });
  }
});
</script>