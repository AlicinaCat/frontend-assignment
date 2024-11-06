<script setup>
  import List from './components/List.vue'
</script>

<template>
<div
    class="h-screen w-full overflow-auto flex bg-gradient-to-br from-teal-500 to-slate-700"
  >
    <div class="mx-auto my-auto text-white">
      <h1 class="mx-auto text-center text-3xl font-bold mt-4">
        Albums
      </h1>
      <List />
    </div>
  </div> 
</template>