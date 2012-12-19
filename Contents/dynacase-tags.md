# Dynacase-tags {#dynacase_tags}


Un document peut posséder plusieurs mots-clés.

Pour qu'un document puisse avoir un mot-clé, il faut que la famille de ce document possède la [propriété "TAGABLE"](#tagableProperty).
Une famille ne peut posséder la [propriété "TAGABLE"](#tagableProperty) que si le module dynacase-tags est installé. Dans le cas contraire, l'importation de la famille ne sera pas possible et une erreur sera remontée.

On peut accéder à une interface de gestion de mots-clés de deux façons différentes. Soit en utilisant la [méthode "tag()"](#methodTag) accessible depuis l'instance de l'objet "Doc" du document, soit directement en utilisant les méthodes statiques de la [classe "TagManager"](#tagManager).

Lors de la duplication de documents, les mots-clés ne sont pas copiés, de plus les mots-clés sont indépendants des révisions de documents (un mot-clé posé sur un document sera visible sur toutes les étapes de ce document).

## La propriété TAGABLE {#tagableProperty}

La [propriété "TAGABLE"](#tagableProperty) est une propriété de la famille. Elle est à ajouter dans l'ods de la famille dont on voudra poser des mots-clés sur les documents:

![ Exemple d'un fichier ods avec la propriété TAGABLE ](ods_tag.png)

Cette propriété peut avoir quatre valeurs :

"no" 

: Les documents de la famille ne peuvent pas avoir de mots-clés. Ils ne sont jamais visibles sur le document et il n'y a pas d'interface permettant de les ajouter/supprimer (valeur par défaut) (Les interfaces de gestion des mots-clés ne sont pas fournis par dynacase-core. Elles sont disponibles dans le module [dynacase-tags-ui](#dynacase_tags_ui)).

"restricted" 

: Les mots-clés des documents de la famille sont affichés, mais on peut en ajouter ou en supprimer que si on a le droit d'éditer les documents de la famille.

"public" 

: Les mots-clés des documents de la famille sont affichés et on peut en ajouter si on a le droit de voir les documents de la famille. Pour la suppression de mot-clé, il faut toujours avoir le droit d'éditer les documents de la famille.

"free"

: Les mots-clés des documents de la famille sont affichés, et on peut en ajouter ou en supprimer quelque soit nos droits.

## La méthode tag() {#methodTag}


Cette méthode est accessible à partir d'une instance de la classe "Doc". Elle permet d'avoir accès à des fonctions de gestion de mot-clé pour le document qui l'appelle. Si le module n'est pas installé et que la méthode est appelée, une exception est envoyée.

Les méthodes vérifient si la famille du document autorise la gestion de mots-clés ([propriété "TAGABLE"](#tagableProperty) différente de "no") et retournent un message d'erreur dans le cas où elle ne l'est pas (une chaîne de caractère précisant que le document ne peut pas avoir de mot-clé).

Si une erreur survient lors de l'utilisation d'une des méthodes, une exception est envoyée. Elles sont du type \Dcp\Exception.

Exemple :

    [php]
        $doc = new_Doc($action->dbaccess, $docid);
    	if ($doc->isAlive()) {
    	    try {
    	        $tags = $doc->tag()->listTag(); //list of tags for document $doc
    	         //Usage example
                if (count($tags) > 0) {
                    $doc->tag()->renameTag($tags[0], "Premier tag");
                    $doc->tag()->setTag("Second tag");
                    $doc->tag()->deleteTag($tags[0]);
                }
    	    } catch (\Dcp\Exception $e) {
    	        $action->exitError($e->getMessage());
    	    }
    	}

Les différentes méthodes accessibles par la [méthode "tag()"](#methodTag) sont :

`listTag()` 

: Retourne un tableau représentant tous les mots-clés du document. Le tableau contient, pour chaque index, un tableau associatif de la forme :

        [php]
        array(
            "tag" => "valeur_du_tag",
            "initid" => "initid_du_document_portant_ce_tag",
            "date" => "date_de_pose_du_tag",
            "fromuid" => "identifiant_de_l'utilisateur"
            );

En cas d'absence de mot-clé sur le document, un tableau vide est renvoyé.

`deleteTag($tag)` 

: Supprime le mot-clé du document ayant pour valeur $tag (une chaine de caractère). Retourne l'objet courant ($this) .

`setTag($tag)` 

: Ajoute le mot-clé $tag (une chaine de caractère) au document. Retourne l'objet courant($this). Si le document possède déjà le même mot-clé, cette méthode retourne l'objet courant ($this), aucune exception n'est envoyée, et le mot-clé n'est pas ajouté (dans ce cas le document n'est pas modifié).

`renameTag($currentTag, $newTag)`

: Change la valeur du mot-clé du document $currentTag (une chaine de caractère) en $newTag (une chaine de caractère). Renvoie l'objet courant($this). Si $newTag est vide, une exception est renvoyée. Si $newTag et $oldTag ont la même valeur, rien n'est fait et l'objet courant ($this) est renvoyé. Dans ce cas le document n'est pas modifié. Si le mot-clé est modifié, tous les éléments du mot-clé le sont (valeur, initid, date de pose et fromuid).

## Les méthodes statiques de la classe TagManager {#tagManager}


Elles sont accessibles comme n'importe qu'elle méthode statique. Elles permettent une gestion sur tous les mots-clés, indépendamment des documents qui les portent.

Pour chaque méthode, si une erreur est détectée, une exception du type \Dcp\Exception est renvoyée.

Ces méthodes, contrairement aux méthodes liées à la [méthode "tag()"](#methodTag), ne vérifient pas les droits des documents car leur utilisation est indépendante des documents. C'est à l'intégrateur de s'assurer que les utilisateurs utilisant ces méthodes ont le droit de le faire.

Exemple :

    [PHP]
     try {
            switch ($type) {
                case "getAll":
                    $data = TagManager::listAllTags($start, $slice, $sSearch, $sortSearch);
                    break;

                case "getValue":
                     $data = TagManager::listAllTags($start, $slice, $sSearch, $sortSearch);
                     $data = TagManager::listTagsValue($data);
                     break;

                case "getAllAndCount":
                    $data = TagManager::listAllTagsWithNumber($start, $slice, $sSearch, $sortSearch);
                    break;

                case "update":
                    TagManager::renameTagOnAllDocument($oldTag, $tags);
                    break;

                case "deleteAll":
                    TagManager::deleteTagOnAllDocument($tags);
                    break;

                default:
                    $out['error'] = sprintf(_("tagmanagement: wrong action [%s]") , $type);
            }
        }
        catch(\Dcp\Exception $e) {
            $out["error"] = $e->getMessage();
        }

Les différentes méthodes statiques sont :

`listTagsValue(array $tags)` 

: Prend en paramètre un tableau d'information sur les mots-clés (le retour de listTag() par exemple) et retourne un tableau ne contenant que les noms des mots-clés.

`listAllTags($start = 0, $slice = 0, $query = "", $orderby = "")` 

:   *Paramètres* :

   > *    $start : Un entier représentant l'offset de la recherche
   > *    $slice : Un entier représentant la limite de la recherche
   > *    $query : une chaîne de caractère représentant une partie (en commençant par le début) ou toute la valeur du mot-clé recherché
   > *    $orderby : une chaîne de caractères représentant la colonne avec laquelle on veut trier la recherche (mot-clé, date, initid, fromuid)

   *Retour* :

   > *   Un tableau de toutes les informations sur les mots-clés commençant par $query, ordonnés par $orderby, commençant à l'index $start et ayant $slice éléments. Si $slice est égale à zéro, alors il n'y a pas de limite sur le nombre d'éléments renvoyés.

   >     Le tableau contient pour chaque index un tableau associatif de la forme :

            [php]
            array(
                "tag" => "valeur_du_tag",
                "initid" => "initid_du_document_portant_ce_tag",
                "date" => "date_de_pose_du_tag",
                "fromuid" => "identifiant_de_l'utilisateur"
            );


`listAllTagsWithNumber($start = 0, $slice = 0, $query = "", $orderby = "")`

:   *Paramètres* :

   > *    $start : Un entier représentant l'offset de la recherche
   > *    $slice : Un entier représentant la limite de la recherche
   > *    $query : une chaîne de caractère représentant une partie (en commençant par le début) ou toute la valeur du mot-clé recherché
   > *    $orderby : une chaîne de caractères représentant la colonne avec laquelle on veut trier la recherche (mot-clé, date, initid, fromuid)

   *Retour* :

   > *  Un tableau contenant les valeurs des mots-clés ainsi que le nombre de fois où ils sont posés sur différents documents.

   >    La valeur des mots-clés de ce tableau commence par $query, sont ordonnés par $orderby, commence à l'index $start et ont $slice éléments.

   >    Si $slice est égale à zéro, alors il n'y a pas de limite sur le nombre d'éléments renvoyés.

   >    Chaque index est composé d'un tableau associatif de la forme :

            [php]
            array(
                "tag" => "valeur_du_tag",
                "number" => "nombre_de_tag_sur_différents_documents"
            );


`deleteTagOnAllDocument($tag)` 

: Prend en paramètre une chaîne de caractères représentant la valeur du mot-clé que l'on veut supprimer ou un tableau de valeur de mots-clés. Supprime ce mot-clé sur tous les documents. Retourne l'objet courrant ($this).

`renameTagOnAllDocument($tagValue, $newTagValue)` 

: Prend en paramètre la valeur du mot-clé que l'on veut renommer ou un tableau de valeurs de mots-clés, et la nouvelle valeur que l'on veut donner à ce/ces mots-clés. Change la valeur du/des mots-clés $tagValue en $newTagValue sur tous les documents. Retourne l'objet courrant ($this).
