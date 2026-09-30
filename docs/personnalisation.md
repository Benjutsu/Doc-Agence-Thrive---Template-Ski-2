# Personnalisation visuelle et contenu

La personnalisation d'un nouveau site doit distinguer les variables globales, les règles du UI kit et le contenu Twig. Cette séparation permet de changer l'identité d'un site sans casser les composants communs.

## Règle générale : partir d'une base vierge

Le nouveau site doit être installé à partir d'une base de données vierge, sans reprendre la base d'un ancien projet. Une base existante peut conserver des pages, options Carbon Fields, médias, réglages Polylang, URLs de boutique, termes de taxonomie, menus ou informations de contact qui ne sont plus valides.

Après l'installation, vérifier également les contenus importés par défaut et les options du thème. Rechercher les anciens noms de station, noms de société, domaines, numéros de téléphone, adresses, réseaux sociaux et URLs dans le contenu WordPress comme dans les options du thème. Cette vérification évite qu'une ancienne identité réapparaisse dans le header, le footer, les métadonnées ou les liens de réservation.

## Langue de référence du code

La langue source des textes métier déjà écrits dans les templates Twig est principalement l'italien : par exemple `Prenota ora`, `I nostri servizi`, `Il negozio` et `La stazione`. Le code contient aussi des textes temporaires en anglais (`Book online`, `Learn more`, `Online payment methods`) et des textes d'administration en anglais ou en français.

Les textes affichés par le front sont généralement enveloppés dans `__()` avec le text domain `timberrock`. Il faut donc considérer les chaînes présentes dans le code comme les identifiants sources à traduire, et non comme les textes définitifs du nouveau site. Les traductions doivent être gérées avec Loco Translate dans le domaine `timberrock` : générer ou mettre à jour le catalogue, créer les langues nécessaires, puis traduire chaque chaîne. Ne pas traduire les slugs, les clés PHP, les valeurs `winter`/`summer` ni les noms de champs comme `var_site_tel`.

Après une modification d'une chaîne dans Twig, régénérer le catalogue pour qu'elle soit détectée par Loco Translate. Contrôler ensuite le front dans chaque langue, car une chaîne absente du catalogue ou un mauvais text domain empêchera sa traduction.

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

## Vérification des textes de la maquette

La maquette doit être la référence pour tous les textes visibles. Il ne suffit pas de remplacer les `Lorem ipsum` : il faut comparer le rendu avec chaque écran de la maquette.

### Header et footer

Vérifier que les labels du header correspondent exactement à la maquette : noms des rubriques, intitulés du mega-menu, liens de réservation, téléphone et sélecteur de langue. Faire la même vérification dans le footer : colonnes, titres, liens légaux, coordonnées, réseaux sociaux, moyens de paiement et messages de réassurance.

Le menu est construit par `MenuManager.php`, mais ses labels sont affichés par Twig. Une modification du menu doit donc être contrôlée dans le menu desktop, le menu mobile et les deux états de saison.

### Boutons et liens d'action

Contrôler tous les textes de boutons, notamment `Prenota ora`, `Scopri di più`, `Scopri l'estate`, `Scopri l'inverno`, `I nostri servizi invernali`, `I nostri servizi estivi`, `Visualizza su Google Maps`, `Vedi tutto` et `Learn more`. Vérifier à la fois le libellé, la traduction, la casse, la longueur dans le composant et la destination du lien.

Un bouton de réservation n'a pas toujours le même comportement : en hiver, il peut pointer vers l'URL boutique externe ; en été, il ouvre l'iframe. Le texte doit rester cohérent avec cette action dans le header, la hero, les sections, le menu mobile et le footer.

**Exemple :** le bouton « Réserver » du header peut ouvrir `https://booking.example.com/winter` en hiver, mais ouvrir l'iframe en été. Dans les deux cas, le libellé doit être traduit de la même manière et l'action réelle doit être testée.

### Titres, sous-titres et textes codés dans Twig

Comparer les titres et sous-titres codés dans les appels `__()` avec la maquette : titres de hero, surtitres, sections services, équipements, boutique, station, offres, témoignages, blog, contact et FAQ. Remplacer les chaînes de démonstration qui sont déjà présentes dans le code, notamment les `Lorem ipsum`, `LOREM IPSUM`, `LOREM STATION` et les titres génériques.

Les textes visibles dans les templates doivent être remplacés par le contenu du nouveau site. Rechercher au minimum :

- `Lorem ipsum` dans les pages et sections ;
- les labels métier Cortina comme `I nostri servizi`, `Il negozio`, `La stazione` et `Prenota ora` ;
- les textes du footer sur le paiement, l'annulation et les échéances ;
- les textes d'accessibilité et les attributs `alt` ;
- les URLs ou slugs propres à Cortina.

Les textes destinés à être traduits doivent rester dans `__()` ou `{{ __(...) }}` avec le text domain du thème. Les textes éditoriaux variables doivent plutôt venir des champs WordPress/Carbon Fields que d'être codés en dur dans Twig.

## Personnalisation dans le back-office

Avant la recette, parcourir toutes les options du thème et les contenus WordPress dans chaque langue.

### Identité du site

- remplacer le logo principal et le logo mobile dans les assets du thème ;
- renseigner le nom du site dans les réglages WordPress, le titre du site et les éventuels champs SEO ;
- vérifier le favicon, les images de partage, les alt des images et les métadonnées ;
- remplacer les images de hero, de menu, de boutique, de station, de marques et de sections par les visuels du nouveau site.

Le nom du projet ne doit pas seulement être changé dans le back-office : vérifier également les traductions, fichiers de configuration, noms de médias et éventuelles chaînes codées en dur.

### Coordonnées et liens externes

Dans les options Carbon Fields du thème, onglet **Coordonnées**, renseigner et vérifier :

- téléphone et lien d'appel ;
- adresse ;
- coordonnées de carte et lien Google Maps ;
- adresse e-mail.

Dans l'onglet **Réseaux sociaux**, renseigner les URLs Facebook, Instagram et X/Twitter. Tester chaque lien depuis le footer et vérifier qu'il s'ouvre sur le bon compte.

### Boutique et réservation

Dans l'onglet **Boutique**, renseigner une URL de boutique d'hiver et une URL de boutique d'été pour chaque langue Polylang active. Tester les liens depuis le header, le mega-menu, les cartes, les boutons de sections et le footer.

Vérifier également le comportement de réservation de l'été avec l'iframe : URL appelée, formulaire, langue et affichage mobile. Une URL vide ou associée à la mauvaise langue peut produire un bouton sans destination fonctionnelle.

### Saisons

Dans les options **Winter Season** :

- définir la date de début de l'hiver ;
- définir la date de fin de l'hiver ;
- vérifier le cas d'une saison qui traverse le 1er janvier ;
- laisser `Force the season` sur `None` en production, sauf besoin de forcer une saison ;
- utiliser le mode forcé uniquement pour les tests ou la recette.

Attribuer ensuite les termes `winter` et `summer` aux services et équipements. Renseigner les champs d'images, descriptions, offres et contenus propres à chaque saison dans les pages et les modèles concernés.

**Exemple :** un service « Location de skis » reçoit le terme `winter`, tandis qu'un service « Location de VTT » reçoit `summer`. Lorsque `current_season` vaut `summer`, la requête `app.getPostType('service', ..., current_season)` ne doit afficher que les services marqués `summer`.

### Traductions du back-office et du code

Configurer les langues dans Polylang avant de renseigner les URLs par langue. Traduire les pages, services, équipements, FAQ, articles et champs éditoriaux dans chaque langue active. Pour les chaînes du thème, utiliser Loco Translate avec le domaine `timberrock` et vérifier notamment les textes du header, footer, boutons, messages de réservation, textes d'accessibilité et titres de sections.

Une traduction effectuée uniquement dans le BO ne remplace pas une chaîne codée en dur dans Twig si cette chaîne n'est pas enveloppée dans `__()` ou si le text domain est incorrect.

### Traduction des formulaires Gravity Forms

Chaque formulaire Gravity Forms doit être créé et traduit dans toutes les langues actives du site. Pour chaque version, vérifier que :

- le formulaire est associé à la bonne langue ;
- les libellés, descriptions, choix, boutons, messages de validation et notifications sont traduits ;
- les champs obligatoires et les règles de validation correspondent au formulaire de référence ;
- le formulaire est relié au bon formulaire parent.

Le champ **formulaire parent** doit contenir l'ID du formulaire qui existe dans la langue par défaut du site. Le formulaire créé dans la langue par défaut est donc le formulaire parent de toutes les autres traductions. Par exemple, si le premier formulaire créé dans la langue de base possède l'ID `1`, chaque formulaire traduit dans une autre langue doit être associé à l'ID parent `1`.

Cette association doit être vérifiée pour chaque formulaire, même si les traductions sont visuellement correctes. Un mauvais parent peut empêcher la bonne correspondance entre les langues ou provoquer l'affichage du mauvais formulaire dans le parcours de contact ou de réservation.

## Images et contenus saisonniers

Remplacer les images dans `assets/images/` et dans les champs WordPress associés. Garder les conventions de nommage utilisées par les templates, ou modifier simultanément les références Twig :

- `_image_winter` / `_image_summer` pour les bannières ;
- `_image_store_winter_1` et `_image_store_summer_1` pour la boutique ;
- `_image_station_winter_1` et `_image_station_summer_1` pour la station ;
- `header-winter.jpg` et `header-summer.png` pour les visuels du menu.

## Checklist de livraison

La checklist complète est disponible dans [Checklist de livraison](checklist.md).
