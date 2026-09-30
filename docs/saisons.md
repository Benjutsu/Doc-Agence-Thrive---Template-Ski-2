# Gestion des saisons

Le template fonctionne avec deux valeurs normalisées : `winter` et `summer`. Elles sont utilisées par PHP, les taxonomies WordPress, les options Carbon Fields, les classes CSS et les templates Twig.

## Calcul dans PHP

La méthode `getCurrentSeason()` de `src/StarterSite.php` applique cet ordre de priorité :

1. si l'URL contient `?current-season=summer` ou `?current-season=winter`, la valeur est validée, enregistrée dans le cookie `timberrock_current_season` pendant un jour, puis le paramètre est retiré de l'URL par redirection ;
2. si le cookie contient une valeur valide, elle est réutilisée ;
3. si l'option Carbon Fields `var_site_force_season` vaut `summer` ou `winter`, elle est appliquée ;
4. sinon, les dates `var_site_start_winter` et `var_site_end_winter` déterminent automatiquement la saison ;
5. si la période hiver traverse le nouvel an, le code traite le cas où la date de début est supérieure à la date de fin.

Les options sont déclarées dans `src/Model/Options/season_options.php`. Le mode forcé est pratique pour les tests et la recette. En production, il faut laisser `Force the season` à `None` pour utiliser le calendrier.

**Exemple :** avec un début d'hiver au `10/15` et une fin au `04/15`, le site est en mode `winter` du 15 octobre au 15 avril, puis en mode `summer` du 16 avril au 14 octobre. Comme la date de début est supérieure à la date de fin, le code traite correctement le passage du 1er janvier.

Dans `addToContext()`, la valeur calculée est exposée à Twig sous `current_season`. Le même contexte expose aussi `options`, `menu` et l'objet `app`.

## Utilisation dans Twig

Les templates testent la valeur avec une condition simple :

```twig
{% if current_season == "winter" %}
    {# contenu hiver #}
{% else %}
    {# contenu été #}
{% endif %}
```

Les usages principaux sont :

- `base.twig` ajoute `body-winter` ou `body-summer` ;
- `banner-hero.twig`, `services.twig`, `the-shop.twig` et `the-station.twig` sélectionnent les champs image et texte de la saison ;
- `App::getPostType()` filtre les services et équipements avec la taxonomie `season` ;
- `home.twig` n'affiche la section `offres.twig` qu'en hiver ;
- les boutons d'hiver ouvrent le lien boutique externe, tandis que les boutons d'été ouvrent l'iframe de réservation.

## Données à préparer

Pour chaque service et équipement, attribuer le terme `winter` ou `summer` dans la taxonomie `season`. Cette taxonomie est partagée entre les types `service` et `equipment` et reste volontairement indépendante de Polylang.

Pour chaque page qui change avec la saison, renseigner les champs correspondants dans le BO : par exemple `_image_winter`, `_image_summer`, `_shop_description_winter` et `_shop_description_summer`.

Les URLs de boutique sont enregistrées dans les options `var_site_boutique_winter_{lang}` et `var_site_boutique_summer_{lang}`. La fonction Twig `get_permalink_boutique()` gère la langue courante puis renvoie l'URL appropriée.

## Tester une saison

Utiliser `?current-season=winter` ou `?current-season=summer` sur une URL. Après la redirection, le cookie conserve le choix pour les requêtes suivantes. Pour revenir au comportement automatique, supprimer le cookie `timberrock_current_season` ou attendre son expiration, puis désactiver le mode forcé dans les options du thème.

**Exemple de recette :** ouvrir `/services?current-season=summer`, vérifier que l'URL est ensuite nettoyée et que les services d'été sont affichés, puis ouvrir la page d'accueil et confirmer que `body-summer` est présent. Refaire le test avec `current-season=winter` et vérifier l'affichage des offres d'hiver.
