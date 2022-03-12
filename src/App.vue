<script setup></script>

<template>
  <div class="max-h-screen">
    <header class="sticky bg-gray-50 shadow" v-if="$route.meta.title">
      <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
        <h1 class="text-md font-bold leading-tight text-gray-900">
          {{ $route.meta.title }}
          <button>+</button>
        </h1>
      </div>
    </header>
    <main>
      <router-view />
    </main>
  </div>
</template>
