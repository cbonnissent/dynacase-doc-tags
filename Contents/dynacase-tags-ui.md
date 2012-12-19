# Dynacase-tags-ui {#dynacase_tags_ui}

Dynacase-tags-ui est un module pour Platform permettant la gestion, la navigation et l'affichage de mots-clés sur les documents.

Ce module est composé de trois parties :

   > * [La zone_tag_management](#zone_tag_management)
   > * [Le widget d'administration](#admin_tag)
   > * [Le widget de recherche](#search_tag)

Le widget d'administration et le widget de recherche sont construis sur le pattern [widget](http://jqueryui.com/widget/) de [jquery-ui](http://jqueryui.com/).

Ils se comportent donc comme tous les widget jquery-ui (initialisation, options, événements, etc...) et sont basés sur le plugin [datatable](http://www.datatables.net/) de jQuery.

Lors de la création de ces widgets, le plugin génère un objet d'option pour la (ou les) dataTable, et injecte la (ou les) dataTable dans la page courante.

## La zone zone_tag_management {#zone_tag_management}

C'est une zone (au sens dynacase) qui permet l'affichage des mots-clés sur les documents.

Par défaut elle est située dans le pied de page d'un [document pouvant avoir des mots-clés](#tagableProperty).

Si les documents ne possèdent pas les bons droits, le pied de page est celui créer par le controleur du document.

Elle est composée de trois éléments :

![ Exemple de la zone de gestion de mot-clé ](zone_tag_management.png)

   > * Un champ input (qui s'affiche lorsque l'on clique sur le + vert et blanc), permettant d'ajouter de nouveaux mots-clés. Ce champ possède une aide à la saisie, permettant de savoir quels sont les mots-clés déjà existant, et de choisir parmi eux.
   > * Une flèche vers le haut ou le bas (en haut à droite) permettant d'afficher tous les mots-clés. Par défaut la zone aura une taille fixe d'environ deux lignes de mots-clés. Pour voir les autres mots-clés sur ce document il faudra cliquer sur la flèche.
   > * Une liste de mots-clés cliquables, permettant d'accéder à l'interface de recherche par mot-clé et, suivant [les droits](#tagableProperty), un bouton de suppression de mot-clé est disponible.

Bien que la zone soit affichée sur tous les documents que l'on peut mettre de mot-clé dès l’installation du module, les fonctions d'ajout et de suppression de mots-clés ne sont disponibles qu'avec [les bons droits](#tagableProperty).

## Le widget d'administration {#admin_tag}
### Présentation

C'est un widget permettant la gestion et manipulation d'un grand nombre de mots-clés indépendamment des documents.

Il est accessible par défaut depuis le menu administration de Platform.

La sélection des mots-clés est multiple (on peut sélectionner plusieurs mots-clés) et persistante (on peut filtrer la liste des mots-clés et/ou changer de page en sélectionnant différents mots-clés, ils restent sélectionnés même après les changements de pages et filtres).

Les mots-clés sélectionnés apparaissent en dessous des boutons. Ils sont alors cliquables pour accéder au [widget de recherche](#search_tag)

Exemple :

![ Exemple de la zone d'administration de mot-clé ](admin_tag.png)

Il permet :

* de [regrouper plusieurs mots-clés en un seul](#renameAllTags).
* de [supprimer plusieurs mots-clés](#deleteAllTags)
* de [renommer un mot-clé](#renameTag)
* de [chercher parmi les mots-clés](#otherTag) grâce à un système de filtres
* de voir [sur combien de documents différents sont posés les mots-clés](#otherTag)

### Mode d'emploi
#### Dépendances

L'intégration du plugin d'administration requiert l'ajout dans la page web des composants suivants:

* **jquery**: `lib/jquery/jquery.js`
* **jquery-ui**: `lib/jquery-ui/js/jquery-ui.js` & `lib/jquery-ui/css/smoothness/jquery-ui.css`
* **jquery dataTables**: `lib/jquery-dataTables/js/jquery.dataTables.min.js` & `lib/jquery-dataTables/css/jquery.dataTables_themeroller.css`
* **jquery tagadmin**: `TAGMANAGEMENT:jquery.tagadmin.js` & `TAGMANAGEMENT:admin_tag.css`. Ces fichiers doivent être parsés par dynacase-core pour pouvoir fonctionner.

#### Droits

L'utilisateur souhaitant avoir accès au widget tagadmin doit avoir l'ACL : TAGMANAGEMENT_ADMIN de l'application TAGMANAGEMENT

#### Initialisation
Idéalement l'initialisation du plugin doit avoir lieu sur l'évènement *ready* de la page en cours pour permettre à la DOM et aux dépendances d'être chargées au préalable.

L'initialisation du widget tagadmin se fait sur une balise `<table>`, celle-ci doit être pré-existante à l'initialisation. On doit ensuite la sélectionner à l'aide de jQuery et l'initialiser. Il a aussi besoin de trois `<button>` et une `<div>` pour utiliser toutes ses fonctionnalitées.


Exemple d'initialisation du widget tagadmin complet :

Fichier html
: Exemple:

    [html]
     [ZONE CORE:HTMLHEAD]
     <div class="ui-widget">
         <button type="button" disabled="disabled" id="regroupTag"
                 title="[TEXT:TAG:regroup]">[TEXT:TAG:regroup]
         </button>
         <button type="button" disabled="disabled" id="deleteTag"
                 title="[TEXT:TAG:delete]">[TEXT:TAG:delete]
         </button>
         <button type="button" disabled="disabled" id="clearSelection"
                 title="[TEXT:TAG:clear selection]">[TEXT:TAG:clear selection]
         </button>
         <div class="ui-widget">
             <div style="float:left;height: 16px; margin: 1px 0 5px 5px;">[TEXT:TAG:Selected tags:]</div> <div id="help_tag_recap"></div>
         </div>
         <table cellpadding="0" cellspacing="0" border="0" class="display" id="admin_tag">
             <thead class="ui-widget-header">
                 <tr id="header">
                     <th>
                         <input type="text" title="[TEXT:TAG:Tag name search]" id="tag_name_search_header" name="tag_name"
                                placeholder="[TEXT:TAG:enter tag name]" class="elemWithTooltip searchField" value=""/>
                     </th>
                     <th></th>
                 </tr>
                 <tr id="column_name">
                     <th class='name_display'>[TEXT:TAG:Tag name]</th>
                     <th class='number_display'>[TEXT:TAG:Number Tag]</th>
                 </tr>
             </thead>
             <tbody class="ui-widget-content">

             </tbody>
             <tfoot class="ui-widget-header">
                 <tr id="footer">
                     <th>
                         <input type="text" title="[TEXT:TAG:Tag name search]" id="tag_name_search_footer" name="tag_name"
                                placeholder="[TEXT:TAG:enter tag name]" class="elemWithTooltip searchField" value=""/>
                     </th>
                     <th></th>
                 </tr>
             </tfoot>
         </table>
         <div id="tagAdministrationModalBox">[TEXT:Overlay]</div>
     </div>
     [ZONE CORE:HTMLFOOT]

Fichier php
: Exemple:

    [php]
    function admin_tag(Action & $action)
    {
        $action->parent->addJsRef("lib/jquery/jquery.js");
        $action->parent->addJsRef("lib/jquery-ui/js/jquery-ui.js");
        $action->parent->addCssRef("lib/jquery-ui/css/smoothness/jquery-ui.css");
        $action->parent->addJsRef("lib/jquery-dataTables/js/jquery.dataTables.min.js");
        $action->parent->addCssRef("lib/jquery-dataTables/css/jquery.dataTables_themeroller.css");
        $action->parent->addCssRef("TAGMANAGEMENT:admin_tag.css", true);
        $action->parent->addJsRef("TAGMANAGEMENT:jquery.tagadmin.js", true);
        $action->parent->addJsRef("TAGMANAGEMENT/Layout/admin_tag.js");
    }

Fichier js
: Exemple:

    [javascript]
    $(function () {
        $("#admin_tag").tagadmin({
            "searchField":".searchField",
            "dialog":"#tagAdministrationModalBox",
            "regroupButton":"#regroupTag",
            "deleteButton":"#deleteTag",
            "clearSelectionButton":"#clearSelection"
        });
    });

##### Options de configuration

searchField
:   sélecteur vers le/les champs qui servent au filtrage de la dataTable

dialog
:   sélecteur vers l'élément permettant l'affichage des messages par overlay

regroupButton
:   sélecteur vers le bouton qui est utilisé pour [regrouper plusieurs mots-clés en un seul](#renameAllTags).

deleteButton
:   sélecteur vers le bouton qui est utilisé pour [supprimer plusieurs mots-clés](#deleteAllTags).

clearSelectionButton
:   sélecteur vers le bouton qui est utilisé pour [réinitialiser la sélection](#otherTags).


dataTableOptions
:   Options bas niveau de la dataTable (voir la [page d'aide de dataTable](http://www.datatables.net/usage/options)).

#### Méthodes associées

destroy
:   Permet de détruire la table.
    La table est alors supprimée (la balise reste en place, mais son contenu est vidé),

refresh
:   Permet de rafraîchir la table.
    Celle-ci conserve son état courant (page en cours, tri, filtre), mais recharge ses données en effectuant une requête serveur.

initAdminTag
:   Permet d'initialiser les événements liés à la table.
    Cette méthode appelle refresh

gettingSelectedRow
:   Permet d'avoir une liste des mots-clés sélectionnés dans la table.
    Retourne un tableau associatif contenant :

   > *      *"listOfName"*: chaine de caractère listant les mots-clés sélectionnés (de la forme "- nom_du_mot-clé<br/>- nom_du_deuxième_mot-clé..."
   > *      *"counTags"*: nombre de mots-clés sélectionnés

updateRow
:  Permet de mettre à jour une ligne de la table.

   Prend en paramètre l'ancienne valeur du mot-clé ainsi que sa nouvelle valeur.

   La table est recréée entièrement avec les nouvelles informations et les sélection sont perdus.

Ces méthodes sont appelées avec la syntaxe suivante:

    [javascript]
    $("#admin_tag").tagadmin("<fonction_name>")

### Fonctionnement
#### Regroupement de mots-clés {#renameAllTags}

C'est la fonctionnalité qui permet de renommer plusieurs mots-clés en une seule action.

Utilisation

: 1. Sélectionner tous les mots-clés que l'on veut renommer
  2. Cliquer sur le bouton "Regrouper les mots-clés en un seul".
  3. Un overlay apparaît, récapitulant les mots-clés qui vont être changés, ainsi que le nombre de documents différents affectés.
  4. Entrer la nouvelle valeur que l'on veut donner à ses mots-clés dans le champ input de l'overlay.
  5. Cliquer sur le bouton "Regrouper les mots-clés" de l'overlay,
  6. Tous les mots-clés sélectionnés, sur tous les documents qui les possèdent, sont renommés.

   Cette action est irréversible.

   En cas d'erreur, un message expliquant l'erreur appraitra à l'écran, et les mots-clés ne seront pas renommés.

Exemple :
: ![ Exemple de regroupement de mots-clés ](admin_tag_regroup.png)

#### Suppression de plusieurs mots-clés {#deleteAllTags}

C'est la fonctionnalité qui permet de supprimer un ou plusieurs mots-clés sur tous les documents qui les portent.

Utilisation

: 1. Sélectionner les mots-clés que l'on veut supprimer
  2. Cliquer sur le bouton "Supprimer les mots-clés".
  3. Un overlay s'ouvre, récapitulant la liste des mots-clés qui vont être supprimés ainsi que le nombre de documents différents affectés.
  4. Cliquer sur le bouton "Supprimer les mots-clés" de l'overlay
  5. Les tags sélectionnés sont supprimés de tous les documents qui les possèdent.

   Cette action est irréversible.

   En cas d'erreur, un message expliquant l'erreur appraitra à l'écran, et les mots-clés ne seront pas supprimés.

Exemple :
: ![ Exemple de suppression de mots-clés ](admin_tag_delete.png)

#### Renommer un mot-clé {#renameTag}

C'est la fonctionnalité qui permet de renommer un mot-clé sur tous les documents sur lesquels il est posé.

Utilisation

: 1. Double cliquer sur le mot-clé dont on veut changer le nom.
  2. Un champ input contenant la valeur actuel du mot-clé apparaît.
  3. Rentrer une nouvelle valeur pour le mot-clé dans ce champ.
  4. Appuyer sur la touche entrée pour valider la saisie.
  5. Pour annuler la modification, cliquer en dehors du champ input avant d'avoir validé la nouvelle valeur.

Une fois la nouvelle valeur validée, on ne peut plus annuler la modification.

Exemple :
: ![ Exemple de renomage de mots-clés ](admin_tag_rename.png)

#### Autres fonctionnalités {#otherTag}

On peut aussi ordonner la liste par ordre alphabétique, par ordre alphabétique inversé, par nombre de documents qui portent le mot-clé croissant ou décroissant, en cliquant sur les intitulés des colonnes correspondantes.

Comme la sélection est persistante, un bouton "Réinitialiser la sélection" est disponible pour remettre la sélection à zéro.

On peut filtrer par nom de mot-clé en tapant le nom recherché dans le champ en haut ou en bas de la colonne de mot-clé, puis en validant.

## Le widget de recherche {#search_tag}
### Présentation

C'est un widget permettant la navigation par mot-clé.

On peut, comme avec le [widget d'administration](#admin_tag), voir le nombre de documents qui possèdent un mot-clé, mais en plus, avoir le titre de ces documents, un lien vers ces documents et un mécanisme de tri et de filtre parmi ces documents.

Il est accessible en cliquant sur un mot-clé dans un document, ou sur la liste récapitulative du [widget d'administration](#admin_tag).

On peut l'intégrer dans d'autre page à l'aide d'iframe (la page possédant déjà son propre header)


Exemple :

![ Exemple de l'interface de recherche de mots-clés ](search_tag.png)


Il est composé de deux parties :

* à gauche, un équivalent de la [table d'administration](#admin_tag) permettant les mêmes fonctions de filtre et de tri, mais ne permettant de sélectionner qu'une ligne à la fois, et ne permettant pas le renommage/suppression/regroupement.
* à droite , une grille affichant les titres des documents possédants le mot-clé sélectionné. Cette grille permet de filtrer les résultats par titre de documents. Elle permet aussi d'afficher un document en cliquant sur l’icône de la colonne de droite. Le document est affiché dans un overlay et permet la même gestion documentaire que sous dynacase-core.


Exemple :

![ Exemple de recherche de tags ](search_tag_exemple.png)

### Mode d'emploi
#### Dépendances

L'intégration du plugin de recherche requiert l'ajout dans la page web des composants suivants:

* **json2**: `lib/data/json2.js`
    cet élément est nécessaire pour faire fonctionner le plugin sur les navigateurs ne possédant pas l'objet `JSON`.
* **jquery**: `lib/jquery/jquery.js`
* **jquery-ui**: `lib/jquery-ui/js/jquery-ui.js` & `lib/jquery-ui/css/smoothness/jquery-ui.css`
* **jquery dataTables**: `lib/jquery-dataTables/js/jquery.dataTables.min.js` & `lib/jquery-dataTables/css/jquery.dataTables_themeroller.css`
* **jquery tagadmin**: `TAGMANAGEMENT:jquery.tagadmin.js` & `TAGMANAGEMENT:admin_tag.css`. Ces fichiers doivent être parsés par dynacase-core pour pouvoir fonctionner.
* **jquery docgrid**: `DOCUMENT_GRID_UI/Layout/jquery.docGrid.js` & `DOCUMENT_GRID_UI/Layout/jquery.docGrid.css`
* **jquery tagsearch**: `TAGMANAGEMENT:jquery.tagsearch.js` & `TAGMANAGEMENT/Layout/search_tag.css`. Le fichier `jquery.tagsearch.js` doit être parsé par dynacase-core pour pouvoir fonctionner.

#### Droits

L'utilisateur souhaitant avoir accès au widget tagsearch doit avoir l'ACL : TAGMANAGEMENT_NORMAL de l'application TAGMANAGEMENT

#### Initialisation
Idéalement l'initialisation du plugin doit avoir lieu sur l'évènement *ready* de la page en cours pour permettre à la DOM et aux dépendances d'être chargées au préalable.

L'initialisation du widget tagsearch se fait sur une balise `<div>`, celle-ci doit être pré-existante à l'initialisation. On doit ensuite la sélectionner à l'aide de jQuery et l'initialiser.
Cette balise doit contenir au moins deux `<table>` qui doivent être passés à l'initialisation du widget.

Exemple d'initialisation du widget tagsearch complet :

Fichier html
: Exemple:

    [html]
    [ZONE CORE:HTMLHEAD]
    <div id="searchwidget" class="ui-widget">
        <input type="hidden" value="[init_tag]" id="init_tag" class="ui-helper-hidden"/>
        <table cellpadding="0" cellspacing="0" border="0" class="display ui-widget" id="search_document">

        </table>
        <table cellpadding="0" cellspacing="0" border="0" class="display ui-widget" id="search_tag">
            <thead class="ui-widget-header">
                <tr id="column_name_tag">
                    <th class='name_display'>[TEXT:TAG:Tag name]</th>
                    <th class='number_display'>[TEXT:TAG:Number Tag]</th>
                </tr>
            </thead>
            <thead>
                <tr id="header_tag">
                    <th>
                        <input type="text" title="[TEXT:TAG:Tag name search]" id="tag_name_search_header" name="tag_name"
                               placeholder="[TEXT:TAG:enter tag name]" class="elemWithTooltip filter" value=""/>
                    </th>
                    <th></th>
                </tr>

            </thead>
            <tbody class="ui-widget-content">

            </tbody>
            <tfoot class="ui-widget-header">
                <tr id="footer_tag">
                    <th>
                        <input type="text" title="[TEXT:TAG:Tag name search]" id="tag_name_search_footer" name="tag_name"
                               placeholder="[TEXT:TAG:enter tag name]" class="elemWithTooltip filter" value=""/>
                    </th>
                    <th></th>
                </tr>
            </tfoot>
        </table>
    </div>
    [ZONE CORE:HTMLFOOT]

Fichier php
:   Exemple:

    [php]
    function search_tag(Action & $action)
    {
        $action->lay->set("init_tag", $action->getArgument("tag"));
        $action->parent->addJsRef("lib/data/json2.js");
        $action->parent->addJsRef("lib/jquery/jquery.js");
        $action->parent->addJsRef("lib/jquery-ui/js/jquery-ui.js");
        $action->parent->addCssRef("lib/jquery-ui/css/smoothness/jquery-ui.css");
        $action->parent->addJsRef("lib/jquery-dataTables/js/jquery.dataTables.min.js");
        $action->parent->addCssRef("lib/jquery-dataTables/css/jquery.dataTables_themeroller.css");
        $action->parent->addCssRef("TAGMANAGEMENT/Layout/admin_tag.css");
        $action->parent->addJsRef("TAGMANAGEMENT:jquery.tagadmin.js", true);
        $action->parent->addJsRef("DOCUMENT_GRID_UI/Layout/jquery.docGrid.js");
        $action->parent->addCssRef("DOCUMENT_GRID_UI/Layout/jquery.docGrid.css");
        $action->parent->addCssRef("TAGMANAGEMENT/Layout/search_tag.css");
        $action->parent->addJsRef("TAGMANAGEMENT:jquery.tagsearch.js", true);
        $action->parent->addJsRef("TAGMANAGEMENT/Layout/search_tag.js");
    }

Fichier javascript
:   Exemple:

    [javascript]
    $(function () {
        $("#searchwidget").tagsearch({
            "tagTable":"#search_tag",
            docTable:"#search_document",
            searchField: ".filter",
            init_tag: $("#init_tag").val()
        });
    });

##### Options de configuration

Les options en gras sont obligatoires

**tagTable**
:   sélecteur vers la balise `<table>` qui contiendra la partie affichage et recherche des mots-clés

**docTable**
:   sélecteur vers la balise `<table>` qui contiendra la partie affichage et recherche des documents liés aux mots-clés

**searchField**
:   sélecteur vers le/les champs qui servent au filtrage de la dataTable

init_tag
:   valeur de la recherche des mots-clés à l'initialisation du widget

dataTableOptions
:   Options bas niveau de la dataTable (voir la [page d'aide de dataTable](http://www.datatables.net/usage/options)).

#### Méthodes associées

destroy
:   Permet de détruire la table.
    La table est alors supprimée (la balise reste en place, mais son contenu est vidé),

initDocTable
:   Initialise la table qui permet la recherche et l'affichage des documents liés aux mots-clés

iniTagTable
:   Initialise la table qui permet la recherche et l'affichage des mots-clés

getCollection
:   Prend en paramètre la valeur d'un mot-clé et le sélecteur jQuery repésentant le tableau qui contient la partie affichage et recherche des documents liés aux mots-clés.
    Rafraichi le tableau passé en paramètre avec les documents qui possèdent le mot-clé passé en paramètre.

Ces méthodes sont appelées avec la syntaxe suivante:

    [javascript]
    $("#searchwidget").tagsearch("<fonction_name>");
