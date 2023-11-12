<script setup>
const props = defineProps({ repos: Object });
</script>

<template>
  <div
    v-for="github in props.repos?.viewer.repositories.nodes"
    :key="github.id"
    :class="props.class"
    class="p-8 border-4 my-4 rounded-lg bg-gray-50 shadow-xl hover:bg-slate-200"
  >
    <a :href="github.url" target="_blank">
      <h2 class="text-2xl text-indigo-800 font-semibold mb-2 hover:underline">
        {{ github.name }}
      </h2>
    </a>
    <p>{{ github.description }}</p>
    <div class="mt-4">
      <Icon name="fontisto:star" size="1.1rem" class="text-indigo-700" />
      Stars: {{ github.stargazers.totalCount }}
      <Icon name="system-uicons:branch" size="1.1rem" class="text-indigo-700" />
      Forks: {{ github.forks.totalCount }}
      <Icon name="system-uicons:eye" size="1.1rem" class="text-indigo-700" />
      Watchers: {{ github.watchers.totalCount }}
    </div>
  </div>
</template>
