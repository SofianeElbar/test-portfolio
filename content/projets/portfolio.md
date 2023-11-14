---
title: Ce portfolio
description: Porfolio regroupant quelques-uns de mes projets.
date: 2023-02-05
cover: portfo.jpg
---

# Portfolio regroupant une partie de mes projets 🖈

## Utilisation du module Nuxt Content 📜

<font color="black">Je voulais concevoir ce portfolio de telle sorte à ce qu'il puisse me permettre de rentrer dans le détail technique des diverses réalisations que j'ai pu prendre en charge. Je me suis tourné vers le framework Nuxt3 de Vue car au delà de sa faculté à générer des pages statiques côté serveur pour gagner en fluidité, ce dernier me permettait grâce à son module Nuxt Content de personnaliser de manière simple le contenu de mes articles en utilisant une logique de rédaction se rapporochant d'un CMS. Cet outil m'a permis notamment de prendre en charge la syntaxe Markdown et d'afficher du code colorisé dans des blocs.

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/markd.jpg" alt="markdown syntax"></img><figcaption><center><font color="black">La syntaxe markdown ressemble à celle d'un éditeur de texte</center></figcaption></figure>

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/c-highlight.jpg" alt="code highlighting"></img><figcaption><center><font color="black">Syntaxe pour créer un bloc de code</center></figcaption></figure>

## Exploitation de mes données Github grâce à GraphQL 📐

L'une des features que j'ai trouvée amusante à implémenter est le fetch de mes données Github directement sur mon portfolio en exploitant l'API GraphQL fourni par Github. Après avoir créé l'arborescence de mes données à exploiter sur l'explorer de Github, j'ai pu fetcher ces données grâce à une requête gql.

```js
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
```

Cela a été rendu possible grâce au module Nuxt Apollo qui, une fois installé, a permis de me connecter à l'API GraphQL. En suivant la doc j'ai créé un plugin dans lequel j'ai défini une fonction qui met à jour la valeur de mon token, créé grâce à une variable d'environnement, pour pouvoir accéder à mes repositories Github.

```js
export default defineNuxtPlugin((nuxtApp) => {
  const { githubToken } = useRuntimeConfig();
  nuxtApp.hook("apollo:auth", ({ client, token }) => {
    token.value = githubToken;
  });
});
```

Paramétrage de la variable d'environnement dans nuxt.config :

```js
 runtimeConfig: {
    githubToken: process.env.GITHUB_TOKEN,
  },
```
