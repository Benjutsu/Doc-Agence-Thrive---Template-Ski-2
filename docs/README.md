# Template ski 2

> **Référence GitLab** : le template 2 se trouve sur la branche `main` du dépôt [AgenceThrive/cortina-pro-sport](https://gitlab.com/AgenceThrive/cortina-pro-sport/-/tree/main).

## Présentation

Le template 2 est issu du site Cortina, développé avec Bedrock, WordPress, Timber 2 et Twig. Le projet a été standardisé pour fournir une base réutilisable aux futurs sites de stations, boutiques de ski et activités de montagne.

L'objectif n'est pas de dupliquer le contenu de Cortina, mais de conserver son socle technique et fonctionnel :

- une structure de thème Timberrock organisée par pages, sections, cartes et partials ;
- un affichage différent selon la saison hiver ou été ;
- un menu métier construit en PHP et rendu dans Twig ;
- des modèles WordPress pour les services et les équipements ;
- des options administrables via Carbon Fields et compatibles avec Polylang ;
- un système de styles SCSS centralisé, avec un UI kit pour contrôler les composants.

Cette standardisation réduit le temps de démarrage des futurs projets, homogénéise les pratiques de développement et permet de remplacer le contenu et l'identité visuelle sans réécrire les mécanismes communs.

## Démarrer un nouveau site

À partir de `cortina-main`, il faut adapter au minimum :

1. le nom du projet et les variables Bedrock dans `composer.json`, `wp-cli.yml` et la configuration `config/` ;
2. les contenus WordPress, les médias et les options Carbon Fields ;
3. le logo, les images de saison et les images des sections dans `assets/images/` ;
4. les couleurs et la typographie dans `assets/styles/abstracts/_var.scss` et `assets/styles/pages/_ui.scss` ;
5. les textes Twig, en particulier les textes temporaires et les `Lorem ipsum` ;
6. les slugs utilisés par `MenuManager.php` si l'arborescence des pages change.

Les mécanismes transverses sont documentés dans les pages suivantes :

- [Architecture des vues](architecture.md)
- [Gestion des saisons](saisons.md)
- [Menu et navigation](menu.md)
- [Personnalisation visuelle et contenu](personnalisation.md)

## Technologies

- **Bedrock** pour la structure WordPress et Composer ;
- **Timber** pour le contexte PHP et le rendu Twig ;
- **Carbon Fields** pour les options et champs personnalisés ;
- **Polylang** pour les URLs et contenus multilingues ;
- **SCSS** pour les variables, composants et sections ;
- **Stimulus** pour les interactions du menu, du mobile et de la réservation.

La branche `main` du dépôt de référence contient la base du template 2.
