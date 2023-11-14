---
title: Plateforme Tënk
description: Implémentation d'une section commentaires pour le site de Tënk.
date: 2023-06-05
cover: header_tenk_site.jpg
---

# Création d'une section commentaires et maintenance du site Tënk 🖈

## Présentation du projet 📜

<font color="black">Ce projet a nécessité la création d'un back-end pour stocker les données nouvellement créées, en l'occurence le nom, commentaire de l'abonné et sa date de création. Nous avons décidé conjointement avec mon collègue et notre lead developpeur d'opter pour l'implémentaion d'un micro-service hérité de Laraval : Lumen.

Pour le projet front-end, qui est codé en nuxt/vue, nous avons principalement recyclé des composants initialemement présents sur le projet pour insérer les commentaires dans un rail avec un défilement horizontal.

![Espace commentaire](/images/projets/comments.jpg)

De même, nous avons paramétré le Dashboard des modérateurs afin que ces derniers puissent ranger les commentaires émis selon trois catégories d'action : à relire, valider, rejeter.

![Espace modération](/images/projets/moderations.jpg)

Si vous voulez jeter un coup d'oeil au site de Tënk, et pourquoi pas vous abonner, les films valent vraiment le détour, <a href="https://www.on-tenk.com/fr" target="_blank">c'est par ici ;)</a>

## Aspects techniques 📐

La mise en place du projet étant complexe, j'ai décidé pour plus de clarté de me focaliser sur deux aspects uniquement :

- **Création des données côté backend**

Paramétrage de la route en post pour la création d'un commentaire

```php
$router->post('create', ['uses' => 'CommentController@createComment']);
```

Création du controller responsable de la gestion des commentaires et des abonnés

```php
class CommentController extends BaseController
{

  protected CommentRepository $commentRepository;
  protected SubscriberRepository $subscriberRepository;

  public function __construct(CommentRepository $commentRepository, SubscriberRepository $subscriberRepository)
  {
    $this->commentRepository = $commentRepository;
    $this->subscriberRepository = $subscriberRepository;
  }
}
```

Dans ce controller, on paramètre la fonction qui permet la récupération des données nécessaires à la création d'un commentaire, notamment l'identification de l'abonné, le contenu de son commentaire et l'identification du film qu'il commente

```php
function createComment(Request $request)
  {

    $content = $request->input('content');
    $email = $request->input('email');
    $pseudo = $request->input('pseudo');
    $id_film = $request->input('idFilm');
    $film_title = $request->input('filmTitle');
  }
```

Après un jeu de vérification, la dernière étape consiste en la création du commentaire en lui-même grâce à une requête SQL définie dans la classe commentRepository

```php
$result = $this->commentRepository->createComment($content, $id_film,
$film_title, $id_subscriber);
if (!isset($result) || !$result) {
return response()->json(['error' => 'Something went wrong.'], 404);
} else {
return response()->json(['success' => 'Added successfully.'], 200);
}
```

Voici la requête SQL permettant la création d'une ligne dans la table commentaire et d'une ligne lui correspondant dans la table modération

```php
 function createComment($content, $id_film, $film_title, $id_subscriber)
  {

    $cleanContent = $this->sanitizeInput($content);

    // Create comment row in comment table
    $query = "INSERT INTO comments (id_subscriber_fk, id_film, film_title, content) VALUES (:id_subscriber_fk, :id_film, :film_title, :content)";

    $bindings = ['id_subscriber_fk' => $id_subscriber, 'id_film' => $id_film, 'film_title' => $film_title, 'content' => $cleanContent];

    DB::insert($query, $bindings);

    $id_comment = DB::getPdo()->lastInsertId();

    // Create moderation row in moderation table
    $query = "INSERT INTO moderations (id_comment_fk, id_moderator_fk, status) VALUES (:id_comment_fk, :id_moderator_fk, :status)";

    $bindings = ['id_comment_fk' => $id_comment, 'id_moderator_fk' => $id_moderator, 'status' => 'A relire'];

    $inserted = DB::insert($query, $bindings);

    return $inserted;
  }
```

---

- **Fetch des data pour un affichage côté font-end**

Afin de faire communiquer le front de Tënk à notre back, nous avons créé un fichier
typescript dans lequel nous avons déclaré une classe CommentService afin d'interagir avec
notre base de données via des requêtes HTTP.
Voici la requête pour le fetch des avis par film :

```js
export class CommentService {
  static async getComments(filmId: number): Promise<Comment[]> {
    try {
      const response = await fetch(
        `${serveur}/api/film/${filmId}/allvalidcomments`
      );
      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || response.statusText);
      }

      const data = await response.json();
      return data;
    } catch (error) {
      console.log("error: ", error);
      throw error;
    }
  }
}
```

Comme dit plus haut, nous avons recyclé des composants du framework css Buefy présent nativement dans le projet Tënk. L'un d'eux se nomme list-hooper et c'est celui-ci qui nous a permis de faire défiler les commentaires sur un axe horizontal grâce à un caroussel muni de flèches.

```js
<!-- THERE ARE COMMENTS -->
<div v-if="comments.length > 0">
    <!-- Hooper component -->
    <list-hooper :elements="comments">
        <!-- Comment component  -->
        <template v-slot:default="commentProps">
            <film-comment :comment="commentProps.element"></film-comment>
        </template>
    </list-hooper>
</div>
```

Suivant la conditionnelle v-if qu’on a mise en place, le carrousel n'apparaît que s’ il y a des avis qui sont émis.
On a configuré le composant list-hooper via une props nommée elements, afin que la variable comments, qui est liée à tous les avis propres à un film, s’affichent à l’intérieur.

<!-- # Hello, World 👋🏻

This is a paragraph.

This is another paragraph.

![Earth from Space](/images/blog/nasa-Q1p7bh3SHj8-unsplash.jpg)

## This is a heading 2

You can use lists

- list item 1
- list item 2
- list item 3

You can use code blocks

```js
const hello = "world";
console.log(hello);
``` -->
<!-- You can use blockquotes

> This is a blockquote

You can use links

[This is a link](https://www.google.com)

You can use tables

| Column 1 | Column 2 |
| -------- | -------- |
| Row 1    | Row 1    |
| Row 2    | Row 2    |
| Row 3    | Row 3    |

You can use horizontal rules

---

You can use emphasis

**This is bold**

_This is italic_

You can use inline code

`const hello = 'world';`

:::callout{title="Hello World Callout"}
This is a quick tip about markdown
::: -->
