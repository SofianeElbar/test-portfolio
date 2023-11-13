<script setup>
const form = ref(null);

const { data: posts } = await useAsyncData("latest-posts", () =>
  queryContent("/projets").find()
);

const sortedPosts = posts.value.sort((a, b) => {
  const dateA = new Date(a.date);
  const dateB = new Date(b.date);

  return dateB - dateA;
});

const firstThreePosts = sortedPosts.slice(0, 3);

const query = gql`
  {
    viewer {
      repositories(first: 3, orderBy: { field: CREATED_AT, direction: DESC }) {
        totalCount
        nodes {
          id
          name
          createdAt
          description
          url
          forks {
            totalCount
          }
          watchers {
            totalCount
          }
          stargazers {
            totalCount
          }
        }
      }
    }
  }
`;
const { data: repos } = await useAsyncQuery(query);

let nextSectionId = ref("section1");
let direction = ref("down");

// ref pour l'animation des blocs
let section1 = ref(null);
let githubs = ref(null);
let contact = ref(null);

// Fonction pour faire défiler les sections
const scrollSection = () => {
  if (nextSectionId.value === "section1") {
    direction.value = "down";
    nextSectionId.value = "section2";
  } else if (nextSectionId.value === "section2") {
    if (direction.value === "down") {
      nextSectionId.value = "section3";
      direction.value = "down";
    } else {
      nextSectionId.value = "section1";
      direction.value = "down";
    }
  } else if (nextSectionId.value === "section3") {
    if (direction.value === "down") {
      nextSectionId.value = "section4";
      direction.value = "up";
    } else {
      nextSectionId.value = "section2";
      direction.value = "up";
    }
  } else if (nextSectionId.value === "section4") {
    direction.value = "up";
    nextSectionId.value = "section3";
  }
  const sectionElement = document.getElementById(nextSectionId.value);
  sectionElement.scrollIntoView({ behavior: "smooth" });
};

onMounted(async () => {
  const scrollButton = document.getElementById("scrollButton");
  scrollButton.addEventListener("click", scrollSection);

  let observer;

  if (typeof window !== "undefined") {
    observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("show");
        } else {
          entry.target.classList.remove("show");
        }
      });
    });
    // Observer les sections avec l'IntersectionObserver
    observer.observe(section1.value);
    observer.observe(githubs.value);
    observer.observe(contact.value);

    document.querySelectorAll(".project").forEach((item) => {
      observer.observe(item);
    });
  }
});
</script>

<template>
  <section
    id="section1"
    ref="section1"
    class="hide min-h-screen flex flex-col md:flex-row items-center justify-center"
  >
    <div class="md:w-2/4">
      <h1 class="text-4xl font-bold">§ Welcome to my dev journey §</h1>
      <p class="text-base text-gray-900 mt-3 italic">
        Sofiane EL BAR, développeur web et web mobile
      </p>
      <h2 class="text-3xl font-bold mt-8">Qui suis-je ?</h2>
      <p class="text-lg text-justify py-2">
        Développeur web full stack passionné et curieux d'apprendre, je suis à
        la recherche de nouvelles aventures dans le monde du développement. Mon
        enthousiasme pour la collaboration et mon désir de créer des
        applications qui apportent une réelle valeur font de moi un atout
        précieux pour votre équipe. Prêts à coder des solutions innovantes
        ensemble? 💻🚀
      </p>
    </div>
    <video
      src="~/assets/videos/sof.mp4"
      autoplay
      loop
      muted
      class="w-2/4 md:max-w-sm m-28 my-auto rounded-lg shadow-xl"
    >
      Désolé, votre navigateur ne prend pas en charge les vidéos intégrées.
    </video>
  </section>

  <section id="section2" class="min-h-screen flex flex-col justify-center">
    <h2 class="text-3xl font-bold mt-8">Mes derniers projets</h2>
    <div class="grid md:grid-cols-3 pt-8 mb-10 gap-10">
      <Post class="project hide" :posts="firstThreePosts" />
    </div>
  </section>

  <section id="section3" class="min-h-screen flex flex-col justify-center">
    <h2 class="text-3xl font-bold mt-8">Mes derniers Repo Github</h2>
    <div ref="githubs" class="hide grid md:grid-cols-3 pt-8 mb-10 gap-10">
      <Repo :repos="repos" />
    </div>
  </section>

  <section
    id="section4"
    ref="contact"
    class="hide flex flex-col min-h-screen justify-center"
  >
    <div class="mx-auto w-full">
      <h1 class="text-4xl font-medium">Restons en contact</h1>
      <p class="mt-3 text-lg">N'hésitez pas à m'envoyer un mail au besoin</p>
      <Contact :form="form" />
    </div>
  </section>

  <button id="scrollButton" class="fixed bottom-4 left-1/2">
    <img
      :src="
        direction === 'down' ? '/images/down-arrow.svg' : '/images/up-arrow.svg'
      "
      alt="Scroll arrow"
      class="w-12 h-12 md:w-16 md:h-16 orange-icon"
    />
  </button>
</template>

<style>
.hide {
  opacity: 0;
  filter: blur(10px);
  transform: translateX(-100%);
  transition: 1s;
}
.show {
  opacity: 1;
  filter: blur(0);
  transform: translateX(0);
}

.project:nth-child(2) {
  transition-delay: 150ms;
}
.project:nth-child(3) {
  transition-delay: 200ms;
}

@media (max-width: 600px) {
  .orange-icon {
    filter: brightness(0) saturate(100%) invert(29%) sepia(99%) saturate(6703%)
      hue-rotate(360deg) brightness(102%) contrast(101%);
  }
}
</style>
