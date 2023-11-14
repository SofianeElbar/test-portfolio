---
title: Projet de blog sur l'IT
description: Blog qui permettra à terme d'échanger sur le futur du web.
date: 2023-01-24
cover: b.jpg
---

# 📌 Projet de blog pour se tenir informé de l'actualité du web

## 📜 Présentation du projet

<font color="black">Ceci est un projet personnel que j'ai utilisé afin de m'initier à la stack Node.js. L'idée principale est de créer des articles et de les stocker sur une base de données hébergée sur MongoDB Atlas. Le projet est encore en développement.

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/b-accueil.jpg" alt="tv-app-search"></img><figcaption><center><font color="black">Accueil du blog</center></figcaption></figure>

<figure><img style="display: block; margin-left: auto; margin-right: auto" src="/images/projets/b-create.jpg" alt="tv-app-search"></img><figcaption><center><font color="black">Formulaire pour la création d'un article</center></figcaption></figure>

## 📐 Aspects techniques

Pour faciliter la gestion de mon backend, j'ai utilisé le framework Express.js ainsi que l'Object data Modeling (ODM) Mongoose.js afin de "modeler" la structure de mes données. Voici comment j'ai défini l'Object Relational Mapper (ORM) pour la structure des données relatives aux articles qui doivent apparaître sur le blog.

```js
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

const projectSchema = new Schema(
  {
    title: {
      type: String,
      required: true,
    },
    snippet: {
      type: String,
      required: true,
    },
    body: {
      type: String,
      required: true,
    },
  },
  { timestamps: true }
);

const Project = mongoose.model("Project", projectSchema);
module.exports = Project;
```

J'ai défini par la suite mes routes auxquelles j'ai affecté mon controller projectController. Voici un exemple avec la route permettant la création d'un nouvel article.

```js
const express = require("express");
const projectController = require("../controllers/projectController");
const router = express.Router();

router.post("/", projectController.project_create_post);

module.exports = router;
```

Je configure ensuite la requête dans la logique de mon controller pour la création de l'article.

```js
const Project = require("../models/project");

const project_create_post = (req, res) => {
  const project = new Project(req.body);

  project
    .save()
    .then((result) => {
      res.redirect("/projects");
    })
    .catch((err) => {
      console.log(err);
    });
};

module.exports = {
  project_create_post,
};
```
