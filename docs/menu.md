# Menu et navigation

Le menu du template est généré par `cortina-main/web/app/themes/timberrock/src/Tools/MenuManager.php`. Il ne dépend pas d'un menu WordPress configuré dans l'administration : PHP construit une structure de données cohérente, puis Twig se charge de l'affichage.

## Construction de la structure

`MenuManager::getMenu()` retourne deux ensembles :

```php
[
    'header' => [...],
    'footer' => [...],
]
```

Le header contient trois entrées : `winter`, `summer` et `about`. Chaque entrée possède notamment une clé, un label, une image, un lien et des colonnes de liens.

- `buildSeasonItem()` construit une entrée de saison, récupère jusqu'à trois services portant le terme de saison et ajoute une colonne de conseils ;
- `buildAboutItem()` regroupe les liens vers la boutique, la station et le blog ;
- `getAdviceColumn()` ajoute les liens FAQ, contact et téléphone ;
- `getSeasonVisual()` associe `header-winter.jpg` à l'hiver et `header-summer.png` à l'été.

Les services sont interrogés par type `service` et taxonomie `season`. Leurs URLs ajoutent `current-season` et une ancre correspondant au slug du service.

**Exemple :** un service nommé « Location de skis » dont le slug est `location-skis` génère un lien de la forme `/services?current-season=winter#location-skis`. Le menu ouvre donc la bonne saison et positionne la page sur le bon service.

Le footer est organisé en quatre colonnes : hiver, été, informations et contact. Les liens de pages passent par `getPageLinkByPath()`, qui retrouve une page par son slug et demande à Polylang la version traduite si elle existe.

## Rendu dans Twig

`partials/header.twig` inclut `partials/menu.twig` et lui transmet `menu`. Le fichier `menu.twig` parcourt `menu.header`, affiche les colonnes et ajoute les contrôleurs Stimulus `menu` et `mega-menu` pour ouvrir et fermer le mega-menu.

`partials/menu-mobile.twig` réutilise la même structure pour l'affichage mobile. `partials/footer.twig` parcourt `menu.footer` et rend ses colonnes.

Les boutons de réservation suivent la logique saisonnière :

- hiver : lien externe obtenu par `get_permalink_boutique('winter')` ;
- été : ouverture de l'iframe via l'action Stimulus `iframe-booking#open`.

## Éléments à adapter pour un nouveau site

Les slugs suivants doivent exister, ou être modifiés dans `MenuManager.php` : `a-propos`, `services`, `faq`, `contact`, `the-shop`, `the-station` et `blog`. Les images `header-winter.jpg`, `header-summer.png` et `header-about.png` doivent également être remplacées ou conservées avec ces noms.

Les labels traduisibles sont écrits avec le text domain `timberrock`. Il faut conserver ce text domain si le système de traduction du thème est conservé, ou le remplacer partout si le thème reçoit un nouveau domaine.

**Exemple de contrôle :** si la page `the-shop` est renommée en `la-boutique` sans modifier `getPageLinkByPath('the-shop')`, le lien du menu peut retomber sur la page d'accueil. Il faut alors soit conserver le slug attendu, soit adapter le slug dans `MenuManager.php` et tester le lien dans chaque langue.
