<script setup>
const { data: posts } = await useAsyncData("posts", () =>
  queryContent("/projets").find()
);

const sortedPosts = posts.value.sort((a, b) => {
  const dateA = new Date(a.date);
  const dateB = new Date(b.date);

  return dateB - dateA;
});
</script>

<template>
  <h1 class="text-3xl my-8">¤ Divers projets</h1>
  <p class="text-lg mb-8">
    Voici une liste non exhaustive de mes projets web, qu'ils soient personnels
    ou professionels
  </p>
  <section class="grid md:grid-cols-3 mt-8 mb-8 gap-10">
    <Post :posts="posts" />
  </section>
</template>
