# Introduction

Cette documentation décrit l'utilisation du module de gestion de tag pour dynacase.
Ce module permet d'ajouter des tags sur les documents de Platform.
Cela permet de pouvoir regrouper plusieurs documents sous un même sujet, et pouvoir retrouver certains documents plus facilement.

Deux modules sont décrits dans cette documentation:

Le module dynacase-tags
: Il contient les fonctions php utile pour gérer les tags. C'est toute la partie serveur.

Le module dynacase-tags-ui
: C'est un ensemble d'IHM qui permettent la gestion des tags. C'est toute la partie client. Ce module fait appelle aux fonctions de dynacase-tags. Les interfaces utilisent le modlue dynacase-document-grid-ui pour s'afficher.
