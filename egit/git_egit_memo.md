# Mémo pour l'utilisation de Git sous Eclipse via [EGit](https://marketplace.eclipse.org/content/egit-git-integration-eclipse)



**Des liens à garder sous la main...**  
*Le Site de référence : [https://git-scm.com](https://git-scm.com) et sa [documentation]((https://git-scm.com/doc)) ainsi que le [Pro Git book (guide officiel) en version française](https://git-scm.com/book/fr/v2)  
Des tutoriels sur git bien illsutrés : [Devenez un gourou du Git (atlassian)](https://fr.atlassian.com/git/tutorials/)  
Des aides-mémoires pour les commandes git : [ici](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf), [là](https://education.github.com/git-cheat-sheet-education.pdf) ou encore [ici](https://zeroturnaround.com/rebellabs/git-commands-and-best-practices-cheat-sheet/) et [là](https://www.git-tower.com/blog/git-cheat-sheet)  
Une Listes d'outils permettant d'utiliser git en mode graphique : [ici](https://git-scm.com/downloads/guis) et [là](https://git.wiki.kernel.org/index.php/InterfacesFrontendsAndTools#Graphical_Interfaces)*



## Créer un dépôt git local sur un projet existant

* Depuis la vue `Package Explorer`, depuis le projet clic droit  : `Team -> Share project `

* Choix de l'emplacement du dépôt git :
	* directement dans le workspace :  
	   -> Cocher **`Use or create repository in parent folder of project`**  
	   -> Vérifier la présence du projet dans la fenêtre  
	   -> Cliquer sur **`Create Repository`** puis **`Finish`**.  

	* en dehors du workspace : renseignez le champs `Repository` 
	

* Depuis la **vue `Package Explorer`**, vérification de la mise en place du gestionnaire de versions : **`monprojet [monprojet NO-HEAD]`** 

## Mettre à jour le fichier `.gitignore`

* Ouvrir le fichier **`.gitignore `** depuis la vue (`Navigator`) (**`Window -> Show View -> Navigator `**)

* Compléter et sauvegarder le fichier  **`.gitignore `** selon les besoins du projet.  
Par défaut pour un projet Java sous Eclipse : 


``` 
   
    # Eclipse    
    .classpath    
    .project    
    .settings/

    # Maven
    target/
   
```
  
Pour en savoir plus sur comment personnaliser le fichier **`.gitignore `** ...  
- [La rubrique gitignore dans la documentation de git](https://git-scm.com/docs/gitignore#_pattern_format)  
- [A collection of useful .gitignore templates](https://github.com/github/gitignore)    
- [A .gitignore file for Intellij and Eclipse with Maven](http://gary-rowe.com/agilestack/2012/10/12/a-gitignore-file-for-intellij-and-eclipse-with-maven/)  *  
  

## Choisir des fichiers à versionner (ou pas ...)

Depuis la **vue `Navigator`** :  

* **une croix** devant icône : fichier ne devant pas être pisté par le gestionnaire de versions  

* **un point d'interrogation** : fichier non encore renseigné par rapport au pistage du gestionnaire de versions.

* **signe `+`** : fichier devant être pisté par le gestionnaire de vesions c-a-d fichier dans l'**index**.  
=> Ajout d'un fichier à l'index : depuis la vue `Navigator`, clic droit : **`Team -> Add to Index `**.


**Remarque:**  
**HEAD** : référence qui pointe sur le commit    
**Index** : référence qui contient la liste des fichiers dont Git doit pister les modifications.    
**Working tree** (arbre de travail) : référence qui contient est la liste de tous les fichiers présents dans votre dépôt, qu’ils soient pistés par Git ou pas.   


## Effectuer un commit (`Team -> Commit... `)

Depuis la vue `Package explorer` :

* clic droit : **`Team -> Commit `.** 
 
* A renseigner :    
	-> **message** (teneur du commit - [un guide de style git](https://github.com/pierreroth64/git-style-guide)).
	Possibilité d'écrire un message de commit sur plusieurs lignes. Toutefois :  
		* la première ligne est le **titre** de votre commit (visible dans la liste des commits)  
	    * les autres lignes fournissent plus de détails (si besoin)       
	-> **auteur** (pas de commit anonyme : adresse mail).    
	-> choix des fichiers à commiter (pour un fichier nouvellement créé, ne pas oublier de le cocher pour l'ajouter dans l**'index'**   

* Valider via `Commit`  
=> Les fichiers qui sont sous contrôle du gestionnaire de versions ont une **cylindre orange** devant leur icône.





## Visualiser les commits


Depuis la vue `Package Explorer`, clic droit **`Team -> Show in History `** pour ouvrir la **vue `History`** 

*Remarque : pour visualiser plusieurs branches (bouton en haut à droite **Show all branches**)*



## Connecter un dépôt distant existant (Github) au dépôt local (Eclipse) (à rréaliser 1 seule fois)

* Ouvrir la vue `Git Repositories` : **`Window -> Show View -> Other... -> Git -> Git Repositories `**

* Depuis la vue `Git Repositories` se positionner sur le **Remotes** du dépôt à connecter puis clic droit : **`Create Remote...`**.

* Paramétrer la fenêtre `New Remote` pour configurer le dépôt distant :

	* choisir un **nom** (par défaut laisser **`origin`**)
	
	* Choix de l'opération à configurer (`push` ou `fetch`)  
	**`Configure Push`** puis **`OK`**
	
	* Paramétrer la fenêtre **`Configure Push`** (Configurer l'option **`push`**) :   
		* A partir de **`Change`** pour ouvrir la fenêtre `Select a URI` :
	  		* dans **`URI`** : **URL HTTPS** du dépôt GitHub (https://github.com/username/depot.git).   
	  		* dans `Authentification` : compte GitHub (**`User`**) et mot de passe (**`Password`**)  
	  		* cocher **`Store in Secure Store`** et **`Finish`**   

		* A partir de **`Advanced...`** pour ouvrir la fenêtre `Configure Push`   
		Pour une configuration simple :  
			* **`Add All Branches Spec`** pour pousser toutes les branches locales  
			* **`Add All Tags Spec`*** pour tous les tags locaux  
			* Vérifiez que le tableau Specifications for push` contient deux lignes, ne chochez rien d'autres  
			* **`Finish`** pour Terminer la configuration du push  

		* Cliquez sur le **`Save`** pour sauvegarder cette nouvelle configuration de `Remotes` (si vous ne voyez pas le bouton `Save`, aggrandissez la fenêtre :smile: ) 

* Vérifier la configuration à partir de la vue `Git Repositories`    
Le noeud `Remotes` doit contenir le **dépôt distant Github avec le nom `origin`** (nom donné lors de la configuration) composé lui-même de **deux sous-noeuds** portant tous deux le nom de  `https://github.com/username/nomdepot.git` : l'un étant précédé d'une petite flêche **verte** (opération **`fetch`**) et l'autre d'une petite flêche **rouge** (opération **`push`**).


## *Pousser* des données du dépôt local d'Eclipse vers le dépôt distant Github via l'opération `push` (ROUGE) <a id="pushEclipseGithub"></a>

Depuis la **vue `Git Repositories`** se positionner sur le **sous-noeud `https://github.com/username/mondepot.git` précédé de la flèche ROUGE**  de `origin` du `Remotes` du projet.

Clic droit : **`Push`**.   
  
Fenêtre de progression suivie de fenêtre `Push Results : testgit-origin` qui indique que l'opération a bien été effectuée (**`master -> master [new branch]`**).

Cliquer sur `OK`.

### 2. *Récupérer* des données du dépôt distant Github vers le dépôt local d'Eclipse via l'opération `fetch`(VERTE) <a id="fetchGithubEclipse"></a>

#### 2.1 Configurer l'opération `fetch` (1 fois)  

Depuis la **vue `Git Repositories`** se positionner sur le **sous-noeud `https://github.com/username/mondepot.git` précédé de la flèche VERTE** de `origin` du `Remotes` du projet.

Clic droit : **`Configure Fetch...`**

* Paramétrage de l'**`URI`** : **`URL HTTPS`** du dépôt GitHub (https://github.com/username/depot.git) (déjà renseigné si configuration de l'opération `push` déjà réalisée).
 
* Paramétrage propre à l'opération(**`Ref mappings`**) 
	* **`Add`** pour ouvrir la fenêtre `Adding a RefSpec for Fetch`  
	* dans `Remote repository`: adresse du dépôt distant sur Github. 
	* dans `source` : `Ctrl+espace ` pour choisir `master`. 
	* `Next`. 

* **`Finish`**

* **`Save`**

	

#### 2.2 Rappatrier les données distantes vers le dépôt local via l'opération `fetch` (VERTE)


Depuis la **vue `Git Repositories`** se positionner sur le **sous-noeud `https://github.com/username/mondepot.git` précédé de la flèche VERTS**  de `origin` du `Remotes` du projet.

Clic droit : **`Fetch`**.   
  
Fenêtre de progression suivie de fenêtre `Fetch Results : testgit-origin` qui indique que les données ont bien été rappatriées (**`master : origin/master [new branch]**).

Cliquer sur `OK`.

#### ... suivi d'un `merge` pour fusionner les données et mettre à jour l'espace de travail local avec les données rappatrié


Ouvrir la **vue `History`** , depuis la vue `Package Explorer` ou `Navigator`, clic droit : `Team -> Show in History `.

Cliquer sur le bouton `Show all branches and tags` de la **vue `History`** (ce bouton est le dernier de la boutons en haut à droite de la vue).

Sélectionner dans la **vue `History`** le commit sur lequel vous souhaitez travailler (dernier commit qui vient du dépôt distant : `origin/master Ajout README`). 

Clic droit depuis ce commit : **`Merge`** puis **`OK`** aux informations données par la fenêtre `Merge Results` pour réaliser l'opération `merge`.

Vérifier le contenu du projet dans la vue `Package Explorer` pour savoir si cette fusion a bien été réalisée.


Remarque : Pour rappatrier les données depuis un dépôt distant : **`fetch` + `merge`** 
(ce qui revient à un **`Fork`**)


## Importer dans Eclipse un projet hébergé sur Github <a id="Github2Eclipse"></a>

Sélectionner **`Import... -> Git -> Projects from Git`** soit à partir d'un **clic droit dans la vue `Package Explorer`**, soit à partir du menu **`File`**

Cliquer sur **`Next`**.

Choisir alors **`Clone URI`** puis **`Next`**.  

A partir de la fenêtre **`Source Git Repository`** :     
 * **`URI`** : adresse suivante du dépôt distant  
 * **Authentification** : **`User`** (compte GitHub) et **`Password`**  
 * Cochez la case **`Store in Secure Store`**   
 * **`Next`** 


A partir de la fenêtre  **`Branch Sélection`** :  
	* Sélectionnez la(les) branche(s) que vous voulez voir apparaître dans votre dépôt local    
	* Cliquez sur **`Next`**   

A partir de la fenêtre  **`Local Destination`** :  
 *  **`Directory`** : indique où importer le dépôt. A personnaliser avec `Browse` pour importer dans le workspace actuel  
 *  **`Next`** 


A partir de la fenêtre **`Select a wizard to use for importing projects`**, choisir comment
importer le(s) projet(s) depuis le dépôt cloné.  
  
 * **Import existing projects** : pour des projets valides du point de vue d'Eclipse (présence d'un fichier **`.project`** à la racine du projet). 
 
 * **Use the New Project wizard** : création d'un nouveau projet à partir des fichiers existants (si présence d'un dossier **`src`**, mais pas de fichier `.project`)

 * **Import as a general project** : **simple importation** du projet dans son état, sans configuration de fichiers annexes comme le classpath, par exemple (**cette option marche dans tous les cas...**)

Cliquez sur **`Next`**, puis sur **`Finish`** (si le nom du projet vous convient).

Remarque : Pour **convertir un simple projet en projet maven** : depuis la vue `Package Explorer`, clic droit sur le nom du projet **`Configure -> Convert to Maven Project`**


## A propos des branches ...


### Créer une branche : **`Team -> Switch to -> New Branch...`** 

Depuis la **vue `Package Explorer`**, clic droit **`Team -> Switch to -> New Branch...`**  

* dans `Source`, sélectionner la **branche à partir de laquelle** on souhaite créer la nouvelle branche (bien souvent `master`)  
* puis saisir le **nom de la nouvelle branche** : `mabranche`   
* laisser **`Checkout new branch`** pour se retrouver sur cette nouvelle branche une fois **`Finish`** cliqué.

Le nom du projet devient : **`nomprojet [nomprojet nombranche]`**
La création de la branche se voie aussi sur la **vue `History`** via un nouveau label **`nombranche``** sur le dernier commit.


### Se déplacer d'une branche à l'autre (`checkout`) :  **`Team-> Switch To`**

Depuis la **vue `Package Explorer`**, clic droit puis **`Team-> Switch To`** et choisir le nom de la branche.

### Comparer le contenu de deux branches (`diff`) :  **`Compare with -> Branch, Tag or Reference`**  

Depuis la **vue `Package Explorer`**, depuis le fichier à comparer, clic droit : **`Compare with -> Branch, Tag or Reference`**  
Sélectionnez la branche à comparer puis **`Compare`**. 

Fermez cette comparaison en cliquant sur la croix de l'onglet `Compare fichier.java Current and xxxxxxx` (où xxxxxxx : numéro de commit visible dans la vue `History`).


#### Fusionner une branche dans `master` (`merge` sans conflit) : **`Team-> merge`**


Pour ***merger*** une branche `mabranche` dans la branche `master`:  

* se placer sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)     
* sélectionner l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionner la **branche à fusionner** dans `master` : **`mabranche`**   
* conserver les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Une fenêtre `Merge Result` s'ouvre indiquant que la fusion s'est bien passée en indiquant que le résultat est bien **`Fast-forward`**. Il ne reste plus qu'à cliquer sur **`OK`**.

Remarque : Les résultats possibles lors de la fusion peuvent être de type `Already-up-to-date`, `Fast-forward`, `Merged`, `Conflicting` ou encore `Failed`.

### Ajouter un tag sur une branche (`tag`)

Depuis la **vue `History`** sur le **commit à tagger** , clic droit: **`Create Tag`**.  
- **`Tag Name`** : nom du tag (`v1.0` par exemple)  
- **`Tag Message`**   
- puis **`Create Tag`**  

Dans la **vue `History`**, une étiquette avec le nom du tag apparaît en tout début du commit taggé.


## Références

Les références consultées pour l'écriture de ce tutoriel sont disponibles :  [ici](git_references.md)

## On en discute ?
Pour les discussions, c'est par [là](https://github.com/iblasquez/tuto_git/issues)  
Pour les propositions de contenu, de modification par [ici](https://github.com/iblasquez/tuto_git/pulls)  
Et bien sûr, n'hésitez pas à personnaliser vos messages avec des [emojis](http://www.webpagefx.com/tools/emoji-cheat-sheet/) :smile:

## Licence

Ce document est placé sous licence CC BY-NC-SA :  
[Creative Commons
Attribution - Pas d'Utilisation Commerciale - Partage dans les Mêmes Conditions](https://creativecommons.org/licenses/by-nc-sa/4.0/)

<img src="https://licensebuttons.net/l/by-nc-sa/3.0/88x31.png" width="100">

En savoir plus sur [les licences Creative Commons](https://creativecommons.org/licenses/?lang=fr-FR) ...

Toutefois, toute personne enseignant au département Informatique de l'IUT du Limousin souhaitant utiliser ce document doit demander une autorisation préalable :smile:




