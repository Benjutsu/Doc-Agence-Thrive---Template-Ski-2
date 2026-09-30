# Architecture des vues

Le thème se trouve dans `cortina-main/web/app/themes/timberrock/`. Les fichiers Twig sont regroupés selon leur rôle afin de pouvoir remplacer une partie de l'interface sans modifier toute la page.

## Organisation de `views`

```text
views/
├── base.twig
├── partials/       éléments communs du site
├── helpers/        petits composants réutilisables
├── cards/          cartes de contenus WordPress
├── sections/       blocs visuels d'une page
└── templates/
    ├── pages/      pages WordPress
    ├── singles/    contenus individuels
    ├── archives/   listes et archives
    └── 404.twig
```

### `base.twig`

`base.twig` est le layout global. Il fournit le HTML de base et les zones `head`, `top_header`, `header`, `breadcrumb`, `content` et `footer`. Il ajoute aussi la classe `body-winter` ou `body-summer` à partir de `current_season`.

Toutes les pages qui étendent ce fichier héritent donc du header, du footer, du fil d'Ariane, des scripts WordPress et du contexte Timber.

### `templates/pages`

Ces fichiers correspondent aux pages fonctionnelles : `home.twig`, `services.twig`, `the-shop.twig`, `the-station.twig`, `contact.twig`, `faq.twig`, etc. Une page assemble les sections et transmet rarement une logique de présentation complexe.

La page d'accueil est orchestrée dans `templates/pages/home.twig`. Son ordre actuel est :

1. header spécifique de la page ;
2. bannière héro ;
3. marques ;
4. services ;
5. réservation ;
6. boutique ;
7. équipements ;
8. avantages ;
9. équipements saisonniers ;
10. station ;
11. offres, uniquement en hiver ;
12. témoignages ;
13. derniers articles.

Pour modifier l'ordre ou retirer un bloc de la page d'accueil, c'est ce fichier qu'il faut modifier. Pour modifier le contenu ou le HTML d'un bloc, il faut intervenir dans le fichier correspondant de `sections/`.

### `sections`

Une section représente un bloc complet de page : `banner-hero.twig`, `services.twig`, `equipments.twig`, `the-shop.twig`, `the-station.twig`, `reservation.twig`, etc. Les sections utilisent `current_season` pour choisir les images, les contenus et les liens de réservation.

### `partials`

Les partials sont les éléments partagés entre plusieurs pages :

- `header.twig`, `menu.twig` et `menu-mobile.twig` pour la navigation ;
- `top-header.twig` pour les dates de saison ;
- `footer.twig` pour les colonnes et les informations légales ;
- `head.twig` pour les métadonnées ;
- `breadcrump.twig` pour le fil d'Ariane ;
- `iframe.twig` pour la réservation intégrée.

Ils sont inclus avec `include('partials/nom.twig')` depuis `base.twig` ou une page.

### `cards` et `helpers`

Les cartes (`service.twig`, `equipment.twig`, `hotel.twig`, etc.) affichent un type de contenu dans un format répétable. Les helpers (`image.twig`, `post-img.twig`, `icon.twig`) encapsulent les traitements communs des images et des icônes.

## Règle de personnalisation

Une nouvelle page doit généralement étendre `base.twig`, puis inclure des sections existantes. Une nouvelle section doit être ajoutée dans `views/sections/` et importée dans le template de page concerné. Les données doivent rester dans le contexte Timber, les contrôles de saison et les URLs métier étant déjà fournis par PHP.