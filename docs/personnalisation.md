# Personnalisation visuelle et contenu

La personnalisation d'un nouveau site doit distinguer les variables globales, les règles du UI kit et le contenu Twig. Cette séparation permet de changer l'identité d'un site sans casser les composants communs.

## Couleurs dans `_var.scss`

Le fichier `assets/styles/abstracts/_var.scss` est le point d'entrée des couleurs. Modifier en priorité :

```scss
$primary: #E6192E;
$primary-2: #BC1526;
$secondary: $dark;
$gradient-color: #0D334A;
```

- `$primary` est la couleur d'action principale ;
- `$primary-2` sert aux variantes plus foncées ;
- `$secondary` est la couleur secondaire et reprend ici `$dark` (#0D334A) pour les textes et fonds sombres ;
- `$gradient-color` sert aux dégradés sur les images du thème.

Les variantes alpha (`$primary-10`, `$secondary-50`, `$gradient-color-80`, etc.) sont calculées à partir de ces variables. Il faut donc modifier les variables sources plutôt que chaque occurrence de couleur dans les composants.

Le fichier définit aussi `$body-bg`, `$body-color`, `$link-color`, les couleurs d'état et la map `$colors`. Vérifier le contraste des liens, boutons et textes après chaque changement.

## Typographies dans `_ui.scss`

Les règles de la page UI kit se trouvent dans `assets/styles/pages/_ui.scss`. Elles définissent notamment les tailles et poids de `h1`, `h2`, `h3`, ainsi que les classes `.cta`, `.body-large`, `.body-medium`, `.body-small` et `.strong`.

La famille globale est définie dans `_var.scss` avec `$ff`, actuellement `forma-djr-display`. Pour changer de typographie :

1. charger la nouvelle police ou modifier l'import Typekit en haut de `_var.scss` ;
2. mettre à jour `$ff` ;
3. ajuster dans `_ui.scss` les tailles, graisses, interlignes et le responsive sous `768px` ;
4. vérifier la page `ui-kit` et les titres des bannières, cartes et boutons.

`main.scss` importe `pages/_ui.scss` après les composants et les sections. Les changements du UI kit sont donc disponibles dans la feuille compilée `assets/styles/main.css` après compilation SCSS.

## Textes Twig

Les textes visibles dans les templates doivent être remplacés par le contenu du nouveau site. Rechercher au minimum :

- `Lorem ipsum` dans les pages et sections ;
- les labels métier Cortina comme `I nostri servizi`, `Il negozio`, `La stazione` et `Prenota ora` ;
- les textes du footer sur le paiement, l'annulation et les échéances ;
- les textes d'accessibilité et les attributs `alt` ;
- les URLs ou slugs propres à Cortina.

Les textes destinés à être traduits doivent rester dans `__()` ou `{{ __(...) }}` avec le text domain du thème. Les textes éditoriaux variables doivent plutôt venir des champs WordPress/Carbon Fields que d'être codés en dur dans Twig.

## Images et contenus saisonniers

Remplacer les images dans `assets/images/` et dans les champs WordPress associés. Garder les conventions de nommage utilisées par les templates, ou modifier simultanément les références Twig :

- `_image_winter` / `_image_summer` pour les bannières ;
- `_image_store_winter_1` et `_image_store_summer_1` pour la boutique ;
- `_image_station_winter_1` et `_image_station_summer_1` pour la station ;
- `header-winter.jpg` et `header-summer.png` pour les visuels du menu.

## Checklist de livraison

- les variables `$primary`, `$secondary` et `$gradient-color` correspondent à la charte ;
- la police, les graisses et les tailles ont été contrôlées dans `ui-kit` ;
- aucun `Lorem ipsum` ni label Cortina ne reste dans les vues ;
- les termes `winter` et `summer` sont présents sur les services et équipements ;
- les dates et le mode forcé de saison sont configurés ;
- les URLs boutique existent pour chaque langue ;
- les slugs attendus par `MenuManager.php` sont disponibles ;
- les boutons de réservation, le mega-menu et le menu mobile ont été testés dans les deux saisons.