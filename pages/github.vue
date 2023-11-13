<script setup>
const query = gql`
  {
    viewer {
      repositories(first: 8, orderBy: { field: CREATED_AT, direction: DESC }) {
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
</script>

<template>
  <h1 class="text-3xl my-8">¤ Codes sources de mes projets sur GitHub ¤</h1>
  <p class="text-lg mb-8">
    Si vous désirez jeter un coup d'oeil au code source de mes projets, vous
    êtes sur la bonne page ;)
  </p>
  <section class="grid grid-cols-2 mb-10 gap-10">
    <Repo :repos="repos" />
  </section>
</template>
