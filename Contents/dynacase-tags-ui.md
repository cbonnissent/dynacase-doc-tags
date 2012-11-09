# Dynacase-tags-ui

Dynacase-tags-ui est un module pour dynacase platform permettant la gestion, la navigation et l'affichage de tags sur les documents.

Ce module est composé de trois parties :

* La zone_tag_management
* Le widget d'administration
* Le widget de recherche

## La zone zone_tag_management

C'est une zone (au sens dynacase) qui permet l'affichage des tags sur les documents.

Par défaut elle est située dans le pied de page d'un document pouvant être tagué (voir chapitre 2.1).

Elle est composée de trois éléments :

![ Exemple de la zone de gestion de tag ](zone_tag_management.png)

* Un champ input (qui s'affiche lorsque l'on clique sur le + vert et blanc), permettant d'ajouter de nouveaux tags. Ce champ possède une aide à la saisie, permettant de savoir quels sont les tags déjà existant, et de choisir parmi eux.
* Une flèche vers le haut ou le bas (en haut à droite) permettant d'afficher tous les tags. Par défaut la zone aura une taille fixe d'environ deux lignes de tags. Pour voir les autres tags sur ce document il faudra cliquer sur la flèche.
* Une liste de tags cliquables, permettant d'accéder à l'interface de recherche par tag et, en édition de document, un bouton de suppression de tag est disponible.

Bien que la zone soit affichée sur tous les documents que l'on peut taguer dès l’installation du module, les fonctions d'ajout et de suppression de tags ne sont disponibles qu'avec les bons droits.

Pour une famille possédant la propriété "TAGABLE" à "public", tous les utilisateurs ayant les droits d'accès au module peuvent supprimer et ajouter un tag sur les documents de cette famille.

Pour une famille possédant la propriété "TAGABLE" à "restricted", seul les utilisateurs ayant le droit d'éditer les documents de la famille peuvent ajouter ou supprimer des tags.

## Le widget d'administration

C'est un widget permettant la gestion et manipulation d'un grand nombre de tags indépendamment des documents.

Il est accessible dans le menu administration de Platform.

La sélection des tags est multiples (on peut sélectionner plusieurs tags) et persistante (on peut filtrer la liste des tags et/ou changer de page en sélectionnant différents tags, ils restent sélectionnés même après les changements de pages et filtres.

Exemple :

![ Exemple de la zone d'administration de tag ](admin_tag.png)

Il permet :

* de regrouper plusieurs tags en un seul.
* de supprimer plusieurs tags
* de renommer un tag
* de chercher parmi les tags grâce à un système de filtres
* de voir sur combien de documents différents sont posés les tags

### Regroupement de tags

Cela permet de renommer plusieurs tags en une seule appellation.

Il faut sélectionner tous les tags que l'on veut renommer, puis cliquer sur le bouton "Regrouper les tags en un seul".
Une popup apparaît, récapitulant les tags qui vont être changés, ainsi que le nombre de documents différents affectés.
Cette popup possède un champ input qui permet de rentrer la nouvelle valeur que l'on veut donner à ses tags.
Après avoir cliqué sur le bouton "Regrouper les tags" de la popup, tous les tags sélectionnés, sur tous les documents qui les possèdent, sont renommés.
Cette action est irréversible.

Exemple :

![ Exemple de regroupement de tags ](admin_tag_regroup.png)

### Suppression de plusieurs tags

Cela permet de supprimer un ou plusieurs tags sur tous les documents qui les portent.

Il faut sélectionner les tags que l'on veut supprimer, puis cliquer sur le bouton "Supprimer les tags".
Une popup s'ouvrira, récapitulant la liste des tags qui vont être supprimés ainsi que le nombre de documents différents affectés.
Après avoir cliqué sur le bouton "Supprimer les tags" de la popup, les tags sélectionnés sont supprimés de tous les documents qui les possèdent.
Cette action est irréversible.

Exemple :

![ Exemple de suppression de tags ](admin_tag_delete.png)

### Renommer un tag

Cela permet de renommer un tag sur tous les documents sur lesquels il est posé.

Il faut double cliquer sur le tag dont on veut changer le nom.
Un champ input contenant la valeur actuel du tag apparaîtra.
On peut rentrer une nouvelle valeur pour le tag dans ce champ.
Elle est validée lorsque l'on appuie sur la touche entrée.
Pour annuler la modification, il faut cliquer en dehors du champ input avant d'avoir validé la nouvelle valeur.
Une fois la nouvelle valeur validée, on ne peut plus annuler la modification.

Exemple :

![ Exemple de renomage de tags ](admin_tag_rename.png)

### Autres fonctionnalités

On peut aussi ordonner la liste par ordre alphabétique, par ordre alphabétique inversé, par nombre de documents qui portent le tag croissant ou par nombre de documents qui portent le tag décroissant en cliquant sur les intitulés des colonnes correspondantes.

Comme la sélection est persistante, un bouton "Réinitialiser la sélection" est disponible pour remettre la sélection à zéro.

On peut filtrer par nom de tag en tapant le nom recherché dans le champ en haut ou en bas de la colonne de tag, puis en validant.

L'interface d'administration est disponible, pour ceux qui possèdent les droits, dès l'installation du module.

## Le widget de recherche

C'est un widget permettant la navigation par tag.

On peut, comme avec le widget d'administration, voir le nombre de documents qui possèdent un tag, mais en plus, avoir le titre de ces documents, un lien vers ces documents et un mécanisme de tri et de filtre parmi ces documents.

Il est accessible en cliquant sur un tag dans un document.


Exemple :

![ Exemple de l'interface de recherche de tags ](search_tag.png)


Il est composé de deux parties :

* à gauche, un équivalent de la table d'administration permettant les mêmes fonctions de filtre et de tri, mais ne permettant de sélectionner qu'une ligne à la fois, et ne permettant pas le renommage/suppression/regroupement.
* à droite , une grille affichant les titres des documents possédants le tag sélectionné. Cette grille permet de filtrer les résultats par titre de documents. Elle permet aussi d'afficher un document en cliquant sur l’icône de la colonne de droite. Le document est affiché dans une popup et permet la même gestion documentaire que sous Platform.


Exemple :

![ Exemple de recherche de tags ](search_tag_exemple.png)
