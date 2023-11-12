<script setup>
const props = defineProps({ posts: Array, class: String, post: Object });

function blur(element, transitionFactor) {
  const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  const text = element.dataset.value;

  const handleMouseOver = () => {
    let iterations = 0;

    const interval = setInterval(() => {
      element.innerText = element.innerText
        .split("")
        .map((letter, index) => {
          if (index < iterations) {
            return text[index];
          }
          return letters[Math.floor(Math.random() * 26)];
        })
        .join("");

      if (iterations >= text.length) {
        clearInterval(interval);
      }

      iterations += 1 / transitionFactor;
    }, 30);
  };

  return handleMouseOver;
}

onMounted(() => {
  const cards = document.querySelectorAll(".card");

  cards.forEach((card) => {
    const title = card.querySelector(".title");

    card.addEventListener("mouseover", blur(title, 1));
  });
});
</script>

<template>
  <div
    v-for="post in props.posts"
    :key="post.slug"
    :class="props.class"
    class="card bg-white rounded-lg shadow-xl overflow-hidden transition-transform transform hover:scale-105"
  >
    <NuxtLink :to="post._path">
      <div
        :style="
          'background-image: url(/images/projets/' +
          post.cover +
          '); background-size :cover'
        "
        alt="Blog Post Cover Image"
        class="image w-full h-48"
      ></div>
    </NuxtLink>
    <div class="p-6">
      <h2 class="title text-xl font-bold mb-2" :data-value="post.title">
        {{ post.title }}
      </h2>
      <p class="description text-gray-700 mb-4" :data-value="post.description">
        {{ post.description }}
      </p>
      <NuxtLink
        :to="post._path"
        class="inline-block bg-blue-500 hover:bg-blue-700 text-white py-2 px-4 rounded float-right mb-8"
        >En savoir plus
      </NuxtLink>
    </div>
  </div>
</template>

<style>
.title {
  font-family: "Space Mono", monospace;
}

.card:hover .image {
  animation: pan-image 15s linear infinite;
}

@keyframes pan-image {
  0% {
    background-position: 0% 100%;
    background-size: 200%;
  }

  20% {
    background-position: 100% 0%;
    background-size: 200%;
  }

  20.0001% {
    /* -- View 2 -- */
    background-position: 60% 85%;
    background-size: 200%;
  }

  40% {
    background-position: 49% 100%;
    background-size: 100%;
  }

  40.0001% {
    /* -- View 3 -- */
    background-position: 80% 42%;
    background-size: 150%;
  }

  60% {
    background-position: 84% 33%;
    background-size: 100%;
  }

  60.0001% {
    /* -- View 4 -- */
    background-position: 42% 80%;
    background-size: 150%;
  }

  80% {
    background-position: 33% 84%;
    background-size: 100%;
  }

  80.0001% {
    /* -- View 5 -- */
    background-position: 0% 0%;
    background-size: 300%;
  }

  100% {
    background-position: 100% 0%;
    background-size: 200%;
  }
}
</style>
