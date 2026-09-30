# Checklist de livraison

Cette checklist doit être parcourue avant la mise en ligne du site. Elle couvre la base de données, l'identité visuelle, les textes, les traductions, le back-office, les saisons et la recette fonctionnelle.

## Base et identité

- [ ] La base de données utilisée pour le nouveau site est vierge et ne contient aucun ancien contenu ou réglage persistant.
- [ ] Les anciens noms de projets, noms de stations, domaines, adresses, téléphones et URLs ont été recherchés et supprimés.
- [ ] Le nom du site, le logo desktop, le logo mobile, le favicon et les métadonnées correspondent au nouveau projet.
- [ ] Les images de contenu, de hero, de menu et de paiement correspondent à la maquette.

## Charte graphique

- [ ] `$primary`, `$primary-2`, `$secondary` et `$gradient-color` correspondent à la charte.
- [ ] La police, les graisses, les tailles et les interlignes ont été contrôlés dans `ui-kit` et `_ui.scss`.
- [ ] Les contrastes des boutons, liens, textes sur images et états de navigation sont lisibles.
- [ ] `main.css` a été recompilé après les modifications SCSS.

## Textes et maquette

- [ ] Les labels du header, du mega-menu, du menu mobile et du footer correspondent à la maquette.
- [ ] Les textes de tous les boutons et liens d'action correspondent à la maquette et à leur destination réelle.
- [ ] Les titres, surtitres et sous-titres codés dans les appels `__()` ont été comparés à la maquette.
- [ ] Aucun `Lorem ipsum`, `LOREM IPSUM`, `LOREM STATION` ou label Cortina ne reste dans les vues ou le contenu du BO.
- [ ] Les textes en italien présents dans le code source ont été remplacés ou traduits selon la langue cible.
- [ ] Les textes éditoriaux sont renseignés dans le BO lorsqu'ils doivent être administrables.

## Traductions

- [ ] Les langues Polylang du projet sont configurées avant la saisie des contenus.
- [ ] Les pages, services, équipements, FAQ, articles et champs éditoriaux sont traduits dans toutes les langues actives.
- [ ] Le thème `timberrock` est à jour dans Loco Translate.
- [ ] Les chaînes du code utilisent bien le text domain `timberrock` et ont été traduites et contrôlées dans chaque langue.
- [ ] Chaque formulaire Gravity Forms existe dans toutes les langues actives et possède la langue correspondante.
- [ ] Les textes, choix, validations, boutons et notifications de chaque formulaire Gravity Forms sont traduits.
- [ ] Chaque formulaire traduit possède l'ID du formulaire parent correspondant au formulaire de la langue par défaut (par exemple `1` si le premier formulaire de base porte l'ID `1`).
- [ ] Le header, le footer, les boutons, les messages de réservation et les textes d'accessibilité ont été testés dans chaque langue.

## Back-office et saisons

- [ ] Le téléphone, l'e-mail, l'adresse, les coordonnées Google Maps et les réseaux sociaux sont renseignés et testés.
- [ ] Une URL boutique d'hiver et une URL boutique d'été sont renseignées pour chaque langue active.
- [ ] Les dates de début et de fin de la saison hiver sont correctes et le passage du nouvel an a été vérifié si nécessaire.
- [ ] `Force the season` est sur `None` en production, sauf décision explicite.
- [ ] Les termes `winter` et `summer` sont attribués aux services et équipements.
- [ ] Les champs d'images et de contenus hiver/été sont remplis pour les pages concernées.

## Navigation et recette

- [ ] Les slugs attendus par `MenuManager.php` existent ou ont été adaptés.
- [ ] Le double header de la home correspond à la maquette : header transparent dans la hero puis header classique.
- [ ] Le mega-menu, le menu mobile et le footer affichent les bons liens dans les deux saisons.
- [ ] Les boutons de réservation hiver, les liens boutique été et l'iframe de réservation fonctionnent.
- [ ] Le site a été testé sur desktop et mobile, dans les deux saisons et dans chaque langue.
- [ ] Une recherche finale des anciens noms, `Lorem ipsum` et URLs d'anciens projets ne retourne aucun résultat.
