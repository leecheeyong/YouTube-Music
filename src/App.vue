<script setup>
import Home from './components/Home.vue'
</script>

<template>
    <Suspense timeout="100">
      <Home />
      <template #fallback>
        <h2 class="text-[#e8b7b7]text-4xl font-bold">Please wait...</h2>
      </template>
    </Suspense>
</template>