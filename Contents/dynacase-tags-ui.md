# Dynacase-tags-ui {#dynacase_tags_ui}

Dynacase-tags-ui est un module pour dynacase platform permettant la gestion, la navigation et l'affichage de mots-clé sur les documents.

Ce module est composé de trois parties :

* [La zone_tag_management](#zone_tag_management)
* [Le widget d'administration](#admin_tag)
* [Le widget de recherche](#search_tag)

## La zone zone_tag_management {#zone_tag_management}

C'est une zone (au sens dynacase) qui permet l'affichage des mots-clé sur les documents.

Par défaut elle est située dans le pied de page d'un [document pouvant avoir des mots-clé](#tagableProperty).
Si les document ne possède pas les bon droits, le pied de page sera celui créer par le controleur du document.

Elle est composée de trois éléments :

![ Exemple de la zone de gestion de mot-clé ](zone_tag_management.png)

* Un champ input (qui s'affiche lorsque l'on clique sur le + vert et blanc), permettant d'ajouter de nouveaux mots-clé. Ce champ possède une aide à la saisie, permettant de savoir quels sont les mots-clé déjà existant, et de choisir parmi eux.
* Une flèche vers le haut ou le bas (en haut à droite) permettant d'afficher tous les mots-clé. Par défaut la zone aura une taille fixe d'environ deux lignes de mots-clé. Pour voir les autres mots-clé sur ce document il faudra cliquer sur la flèche.
* Une liste de mots-clé cliquables, permettant d'accéder à l'interface de recherche par mot-clé et, en édition de document, un bouton de suppression de mot-clé est disponible.

Bien que la zone soit affichée sur tous les documents que l'on peut mettre de mot-clé dès l’installation du module, les fonctions d'ajout et de suppression de mots-clé ne sont disponibles qu'avec [les bons droits](#tagableProperty).

## Le widget d'administration {#admin_tag}

C'est un widget permettant la gestion et manipulation d'un grand nombre de mots-clé indépendamment des documents.

Il est accessible depuis le menu administration de Platform. L'utilisateur doit posséder les droits "TAGMANAGEMENT_ADMIN" pour pouvoir l'utiliser.
Elle est aussi accessible à l'adresse "racine_du_contexte/?app=TAGMANAGEMENT&action=ADMIN_TAG".
On peut l'intégrer dans d'autre page à l'aide d'iframe (la page possédant déjà son propre header)

La sélection des mots-clé est multiple (on peut sélectionner plusieurs mots-clé) et persistante (on peut filtrer la liste des mots-clé et/ou changer de page en sélectionnant différents mots-clé, ils restent sélectionnés même après les changements de pages et filtres).
Les mots-clé sélectionnés apparaissent en dessous des boutons. Ils sont alors cliquables pour accéder au [widget de recherche](#search_tag)

Exemple :

![ Exemple de la zone d'administration de mot-clé ](admin_tag.png)

Il permet :

* de [regrouper plusieurs mots-clé en un seul](#renameAllTags).
* de [supprimer plusieurs mots-clé](#deleteAllTags)
* de [renommer un mot-clé](#renameTag)
* de [chercher parmi les mots-clé](#otherTag) grâce à un système de filtres
* de voir [sur combien de documents différents sont posés les mots-clé](#otherTag)

### Regroupement de mot-clés {#renameAllTags}

C'est la fonctionnalité qui permet de renommer plusieurs mots-clé en une seule action.

Utilisation

: 1. Sélectionner tous les mots-clé que l'on veut renommer
  2. Cliquer sur le bouton "Regrouper les mots-clé en un seul".
  3. Un overlay apparaît, récapitulant les mots-clé qui vont être changés, ainsi que le nombre de documents différents affectés.
  4. Entrer la nouvelle valeur que l'on veut donner à ses mots-clé dans le champ input de l'overlay.
  5. Cliquer sur le bouton "Regrouper les mots-clé" de l'overlay,
  6. Tous les mots-clé sélectionnés, sur tous les documents qui les possèdent, sont renommés.

Cette action est irréversible.
En cas d'erreur, un message expliquant l'erreur appraitra à l'écran, et les mots-clé ne seront pas renommés.

Exemple :

![ Exemple de regroupement de mots-clé ](admin_tag_regroup.png)

### Suppression de plusieurs mot-clés {#deleteAllTags}

C'est la fonctionnalité qui permet de supprimer un ou plusieurs mots-clé sur tous les documents qui les portent.

Utilisation

: 1. Sélectionner les mots-clé que l'on veut supprimer
  2. Cliquer sur le bouton "Supprimer les mots-clé".
  3. Un overlay s'ouvre, récapitulant la liste des mots-clé qui vont être supprimés ainsi que le nombre de documents différents affectés.
  4. Cliquer sur le bouton "Supprimer les mots-clé" de l'overlay
  5. Les tags sélectionnés sont supprimés de tous les documents qui les possèdent.

Cette action est irréversible.
En cas d'erreur, un message expliquant l'erreur appraitra à l'écran, et les mots-clé ne seront pas supprimés.

Exemple :

![ Exemple de suppression de mots-clé ](admin_tag_delete.png)

### Renommer un mot-clé {#renameTag}

C'est la fonctionnalité qui permet de renommer un mot-clé sur tous les documents sur lesquels il est posé.

Utilisation

: 1. Double cliquer sur le mot-clé dont on veut changer le nom.
  2. Un champ input contenant la valeur actuel du mot-clé apparaît.
  3. Rentrer une nouvelle valeur pour le mot-clé dans ce champ.
  4. Appuyer sur la touche entrée pour valider la saisie.
  5. Pour annuler la modification, cliquer en dehors du champ input avant d'avoir validé la nouvelle valeur.

Une fois la nouvelle valeur validée, on ne peut plus annuler la modification.

Exemple :

![ Exemple de renomage de mots-clé ](admin_tag_rename.png)

### Autres fonctionnalités {#otherTag}

On peut aussi ordonner la liste par ordre alphabétique, par ordre alphabétique inversé, par nombre de documents qui portent le mot-clé croissant ou par nombre de documents qui portent le mot-clé décroissant, en cliquant sur les intitulés des colonnes correspondantes.

Comme la sélection est persistante, un bouton "Réinitialiser la sélection" est disponible pour remettre la sélection à zéro.

On peut filtrer par nom de mot-clé en tapant le nom recherché dans le champ en haut ou en bas de la colonne de mot-clé, puis en validant.

## Le widget de recherche {#search_tag}

C'est un widget permettant la navigation par mot-clé.

On peut, comme avec le widget d'administration, voir le nombre de documents qui possèdent un mot-clé, mais en plus, avoir le titre de ces documents, un lien vers ces documents et un mécanisme de tri et de filtre parmi ces documents.

Il est accessible en cliquant sur un mot-clé dans un document, ou sur la liste récapitulative du [widget d'administration](#admin_tag).
On peut aussi y accéder par l'url "racine_du_contexte/?app=TAGMANAGMENT&action=SEARCH_TAG".
Pour l'utiliser, il faut posséder les droits TAGMANAGEMENT_NORMAL
On peut l'intégrer dans d'autre page à l'aide d'iframe (la page possédant déjà son propre header)


Exemple :

![ Exemple de l'interface de recherche de mots-clé ](search_tag.png)


Il est composé de deux parties :

* à gauche, un équivalent de la [table d'administration](#admin_tag) permettant les mêmes fonctions de filtre et de tri, mais ne permettant de sélectionner qu'une ligne à la fois, et ne permettant pas le renommage/suppression/regroupement.
* à droite , une grille affichant les titres des documents possédants le mot-clé sélectionné. Cette grille permet de filtrer les résultats par titre de documents. Elle permet aussi d'afficher un document en cliquant sur l’icône de la colonne de droite. Le document est affiché dans un overlay et permet la même gestion documentaire que sous dynacase-core.


Exemple :

![ Exemple de recherche de tags ](search_tag_exemple.png)
