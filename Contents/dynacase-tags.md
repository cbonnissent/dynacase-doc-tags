# Dynacase-tags


Un document peut posséder plusieurs tags.

Pour qu'un document puisse avoir un tag, Il faut que la famille de ce document possède la propriété "TAGABLE".

On peut accéder à une interface de gestion de tag de deux façons différentes. Soit en utilisant la méthode "tag()" accessible depuis l'instance de l'objet "Doc" du document, soit directement en utilisant les méthodes statiques de la classe "TagManager".

Lors de la duplication de documents, les tags ne sont pas copiés.

## La propriété TAGABLE


Cette propriété peut avoir trois valeurs :

"no" 

: Les documents de la famille ne peuvent pas avoir de tags. Ils ne sont jamais visibles sur le document et il n'y a pas d'interface permettant de les ajouter/supprimer (valeur par défaut) (Les interfaces de gestion des tags ne sont pas fournis par dynacase-platform. Elles sont disponibles dans le module dynacase-tags-ui).

"restricted" 

: Les tags des documents de la famille sont affichés, mais on peut en ajouter ou en supprimer que si on a le droit d'éditer le document.

"public" 

: Les tags des documents de la famille sont affichés et on peut en ajouter si on a le droit de voir le document. Pour la suppression de tag, il faut toujours avoir le droit d'éditer le document.

## La méthode tag()


Cette méthode est accessible à partir d'une instance de la classe "Doc". Elle permet d'avoir accès à des fonctions de gestion de tag pour le document qui l'appelle.

Les méthodes vérifient si la famille du document autorise la gestion de tag (propriété TAGABLE différente de "no") et retournent un message d'erreur dans le cas où elle ne l'est pas.

Exemple :

    [PHP]
        $doc = new_Doc($action->dbaccess, $docid);
    	if ($doc->isAlive()) {
    		$tags = $doc->tag()->getTag(); //list of tags for document $doc
    		if (is_array($tags)) {
    			//do something with document tags
    		} else {
    			$action->exitError($tags);
    		}
    	}

Les différentes méthodes accessibles par la méthode tag() sont :

`getTag()` 

: Retourne un tableau représentant tous les tags du document  ou un message d'erreur. Le tableau contient, pour chaque index, un tableau associatif de la forme : `array("tag" => "valeur_du_tag", "initid" => "initid_du_document_portant_ce_tag", "date" => "date_de_pose_du_tag", "fromuid" => "identifiant_de_l'utilisateur");`

`delTag($tag)` 

: Supprime le tag du document ayant pour valeur $tag. Retourne un message d'erreur ou une chaîne vide.

`addTag($tag)` 

: Ajoute le tag $tag au document. Retourne un message d'erreur ou une chaîne vide. Si le document possède déjà le même tag, cette méthode retourne une chaîne vide et le tag n'est pas ajouté.
`renameTag($currentTag, $newTag)`

: Change la valeur du tag du document $currentTag en $newTag. Renvoie une erreur ou une chaine vide. Si $newTag est vide, une erreur sera renvoyée. Si $newTag et $oldTag ont la même valeur, rien ne sera fait et une chaîne vide sera renvoyée.

## Les méthodes statiques de la classe TagManager


Elles sont accessibles comme n'importe qu'elle méthode statique. Elles permettent une gestion sur tous les tags, indépendamment des documents qui les portent.

Exemple :

    [PHP]
    $all_tags = TagManager::getAllTags(); //Array of all tags or string with error message

Les différentes méthodes statiques sont :

`getTagsValue(array $tags)` 

: Prend en paramètre un tableau d'information sur les tags (le retour de getTag() par exemple) et retourne un tableau ne contenant que les noms des tags.

`getAllTags($start = 0, $slice = 0, $query = "", $orderby = "")` 

: Paramètres:
  * $start : Un entier représentant l'offset de la recherche
  * $slice : Un entier représentant la limite de la recherche
  * $query : une chaîne de caractère représentant une partie (en commençant par le début) ou toute la valeur du tag recherché
  * $orderby : une chaîne de caractères représentant la colonne avec laquelle on veut trier la recherche (tag, date, initid, fromuid)

  Retour:
   un tableau de toutes les informations sur les tags commençant par $query, ordonnés par $orderby, commençant à l'index $start et ayant $slice éléments. Si $slice est égale à zéro, alors il n'y aura pas de limite sur le nombre d'éléments renvoyés.
 Le tableau contient pour chaque index un tableau associatif de la forme :
 `array("tag" => "valeur_du_tag", "initid" => "initid_du_document_portant_ce_tag", "date" => "date_de_pose_du_tag", "fromuid" => "identifiant_de_l'utilisateur")`.

`getAllCounts()` 

: Retourne le nombre total de tags différents sur tous les documents de toutes les familles qui en possèdent ou un message d'erreur.

`getAllTagsAndCount($start = 0, $slice = 0, $query = "", $orderby = "")`

: Prend en paramètre facultatif deux entiers représentant l'offset et la limite de la recherche, une chaîne de caractère représentant une partie (en commençant par le début) ou toute la valeur du tag recherché et une chaîne de caractères représentant la colonne avec laquelle on veut trier la recherche (tag, number).
 Retourne un tableau contenant les valeurs des tags ainsi que le nombre de fois où ils sont posés sur différents documents dont la valeur des tags commence par $query, ordonnés par $orderby, commençant à l'index $start et ayant $slice éléments ou un message d'erreur. Chaque index est composé d'un tableau associatif de la forme : `array("tag" => "valeur_du_tag", "number" => "nombre_de_tag_sur_différents_documents")`

`getTagCount($tag)` 

: Prend en paramètre une chaîne de caractères représentant la valeur d'un tag ou un tableau de valeur de tags. Retourne le nombre de documents différents qui portent un de ces tags.

`deleteTagOnAllDocument($tag)` 

: Prend en paramètre une chaîne de caractères représentant la valeur du tag que l'on veut supprimer ou un tableau de valeur de tags. Supprime ce tag sur tous les documents. Retourne une chaîne vide ou un message d'erreur.

`renameTagOnAllDocument($tagValue, $newTagValue)` 

: Prend en paramètre la valeur du tag que l'on veut renommer ou un tableau de valeurs de tags, et la nouvelle valeur que l'on veut donner à ce/ces tags. Change la valeur du/des tags $tagValue en $newTagValue sur tous les documents. Retourne une chaîne vide ou un message d'erreur.
