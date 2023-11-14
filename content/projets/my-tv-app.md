---
title: My-tv-app
description: Application de recherche de ses séries préférées à partir d'une API.
date: 2023-04-24
cover: tv-app.jpg
---

# Application de recherche de séries à partir d'une API 🖈

## Présentation du projet 📜

<font color="black">Le but de l'application est de fetcher les données d'une API en ligne, pour les afficher par la suite dans un template prédéfini. L'application fonctionne en tiroir car elle permet à partir d'une série TV de retrouver l'ensemble des acteurs impliqués ainsi que l'ensemble des projets auxquels ils ont participé.

Pour ce projet, j'ai utilisé la bibliothèque React.

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/tv-search.jpg" alt="tv-app-search"></img><figcaption><center><font color="black">Recherche principale</center></figcaption></figure>

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/tv-actor.jpg" alt="tv-app-search"></img><figcaption><center><font color="black">Recherche par acteurs</center></figcaption></figure>

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/tv-projects.jpg" alt="tv-app-search"></img><figcaption><center><font color="black">Recherche par projets d'acteurs</center></figcaption></figure>

Si vous souhaitez jeter un coup d'oeil à l'application <a href="https://my-web-app-64479.web.app/" target="_blank">c'est par ici ;)</a>

## Aspects techniques 📐

Afin de récupérer les données de l'API sur les séries et les acteurs qui y participent j'ai utilisé le hook de React useEffect. Le hook useState m'a permis quant à lui de déclarer mes deux variables d'état show et actors que je mets respectivement à jour grâce aux fonctions setShow et setActors.

```js
import { useEffect, useState } from "react";
import { useParams, Link } from "react-router-dom";

export function Show() {
  const { id } = useParams();
  const [show, setShow] = useState("");
  const [actors, setActors] = useState([]);

  useEffect(() => {
    fetch(`https://api.tvmaze.com/shows/${id}?embed=cast`)
      .then((response) => response.json())
      .then((data) => {
        setShow(data);
        setActors(data._embedded.cast);
      });
  }, [show]);
}
```

Enfin, je retourne les données dans un template en format JSX.

```js
return (
  <div>
    <h1>{show.name}</h1>
    <div>
      <img
        src={
          show.image?.medium ||
          "http://vignette3.wikia.nocookie.net/lego/images/a/ac/No-Image-Basic.png/revision/latest?cb=20130819001030"
        }
        alt={show.name}
      />
    </div>
    <div dangerouslySetInnerHTML={{ __html: show.summary }} />
    <h2>Actors :</h2>
    {actors.map((actor) => (
      <div>
        <Link to={"/actor/" + actor.person.id}>{actor.person.name}</Link>
      </div>
    ))}
    <h2>Infos :</h2>
    <h4>{show.genres?.join(" / ")}</h4>
    <h4>Rating : {show.rating?.average}/10</h4>
    <h4>Premiered : {show.premiered}</h4>
    <h4>Ended : {show.ended}</h4>
    <h4>Country : {show.network?.country.name}</h4>
  </div>
);
```
