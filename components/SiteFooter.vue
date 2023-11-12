<script setup>
const links = [
  {
    name: "GitHub",
    href: "https://github.com/sofianeElbar",
    icon: "simple-icons:github",
  },

  {
    name: "LinkedIn",
    href: "https://linkedin.com/in/sofiane-el-bar",
    icon: "simple-icons:linkedin",
  },
];
</script>

<template>
  <footer class="bg-gray-800">
    <section
      class="mx-auto max-w-7xl px-2 sm:px-4 lg:py-6 text-gray-300 flex flex-row"
    >
      <p class="flex-1">&copy; Sofiane EL BAR 2023</p>
      <div class="flex flex-row justify-center gap-4 mb-4 mt-2">
        <a
          v-for="link in links"
          :key="link.name"
          :href="link.href"
          target="_blank"
        >
          <Icon :name="link.icon" size="1.5rem" class="text-gray-300" />
        </a>
      </div>
    </section>
  </footer>
</template>
