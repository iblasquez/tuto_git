# Tutoriel de découverte de [Git](https://git-scm.com) (au travers d'[EGit](http://www.eclipse.org/egit/))

Git est un logiciel de gestion de versions décentralisé c-a-d un **DVCS** (**D**ecentralized **V**ersion **C**ontrol **S**oftware). 
> [Un logiciel de gestion de versions est un logiciel qui permet de stocker un ensemble de fichiers en conservant la chronologie de toutes les modifications qui ont été effectuées dessus. Il permet notamment de retrouver les différentes versions d'un lot de fichiers connexes.](https://fr.wikipedia.org/wiki/Logiciel_de_gestion_de_versions)» (Extrait Wikipédia)

Le site de référence de git est : https://git-scm.com  
Pour tout savoir sur git, un guide officiel (le Pro Book Git) est disponible en ligne en version française [ici](https://git-scm.com/book/fr/v2).

Eclipse propose un support natif git au travers du plug-in [EGit - Git Integration for Eclipse](https://marketplace.eclipse.org/content/egit-git-integration-eclipse). Ce plug-in permet entre autres d'utiliser git en local, de pusher (envoyer le contenu d'un projet existant dans Eclipse sur un dépôt distant dans un service d'hébergement de type [Github](https://github.com/), [Bitbucket](https://fr.atlassian.com/software/bitbucket) ou autre), d'importer un projet hebergé sur Github (Bitbucket ou autre) dans Eclipse.


Dans ce tutoriel, nous verrons comment configurer un environnement de développement Java sous Eclipse avec git et plus particulièrement comment :

* [Créer et utiliser un depôt git local sous Eclipse](#gitLocalEclipse)
* [Connecter un projet Eclipse à Github](#eclipse2Github)
* [Echanger des données entre un dépôt local (Eclipse) et un depôt distant (Github)](#interactionEclipseGithub)
* [Importer dans Eclipse un projet hébergé sur Github](#Github2Eclipse)

Nous continuerons par [un petit exercice bien branché qui va fusionner](#exercice),     
et nous terminerons par suggérer quelques lectures pour en savoir plus sur [comment contribuer à un projet existant sur Github ? (fork & co)](#fork),    
et par donner quelques[références](#references).


## Créer et utiliser un depôt git local sous Eclipse <a id="gitLocalEclipse"></a>

Commencez par [créer un projet maven](https://github.com/iblasquez/Back2Basics_Developpement/blob/master/CreerProjetMavenEclipse.md) que vous appelerez `testgit` par exemple.  
Supprimez les fichiers créés par défaut `App.java` et `AppTest.java` pour disposer d'un projet vide.


### 1. Créer un dépôt git local sur un projet existant
Pour créer un dépôt local pour ce projet, placez-vous sur le projet `testgit` dans la vue `Package Explorer`, puis d'un clic droit sélectionnez `Team -> Share project `

Une fenêtre s'ouvre alors pour configurer les options du dépôt Git c-a-d choisir à quel endroit l'on souhaite créer son dépôt Git.  
Remarque : Sous git, on travaille avec un **dépôt** (ou **repository** en anglais).

Cochez la case **`Use or create repository in parent folder of project`** pour créer directement le dépôt local git dans le répertoire du workspace.
Un warning indique alors qu'Eclipse ne recommande pas cette solution, mais pour faciliter la prise en main d'Egit dans ce tutoriel, nous nous en contenterons.  
*Remarque : Notez toutefois, que si vous souhaitez que votre dépôt soit situé en dehors de votre workspace, (par exemple, pour regrouper tous vos projets Git dans un dossier différent de celui des projets pour lesquels vous n'utilisez pas Git), il vous suffit  simplement de renseigner le champs `Repository` de cette fenêtre pour relier votre projet à un dépôt Git local déjà existant (liste déroulante) ou en créer un nouveau via le bouton `Create`.*

Vérifiez que votre projet `testgit` est bien proposé dans la fenêtre, cliquez sur le bouton **`Create Repository`**, puis sur **`Finish`**.  
EGit génèrera automatiquement le chemin du dépôt.  
*Remarque : La solution choisie permet de créer le dépôt à la racine du projet, c-a-d qu'un dossier `.git` est créé à l'intérieur du dossier de votre workspace (vous pouvez le constater par vous même à l'aide d'un explorateur de fichier si vous le souhaitez). Les modifications effectuées en dehors de ce dossier ne seront donc pas visibles pour Git.*

Jetez un petit coup d'oeil dans la **vue `Package Explorer`**, le nom de votre projet a dû être modifié (si ce n'est pas la cas, procédez à un petit rafraîchissement à l'aide de `F5`) et se présente désormais comme : **`testgit [testgit NO-HEAD]`** ce qui signifie que pour l'instant notre dépôt (**`testgit`**) ne contient aucun commit (**`NO HEAD`**).  
La présence du chevron **`[testgit NO-HEAD]`** montre que le projet est géré par un gestionnaire de version

### 2. Mettre à jour le fichier `.gitignore`

Placez-vous sur le projet `testgit`, puis à partir du menu en haut de votre IDE, sélectionnez **`Window -> Show View -> Navigator `**. Un onglet `Navigator` apparait. En se positionnant sur le projet `testgit`, cette vue permet de visualiser, entre autres, trois fichiers cachés : 
   
* `.project` et `.classpath` qui sont des fichiers techniques gérés par Eclipse 
* `.gitignore` qui est un fichier technique utilisé par Git pour spécifier les fichiers et répertoires à ne pas prendre en compte dans la gestion des versions (c-a-d qui ne seront pas versionnés).  

Depuis la vue `Navigator`, cliquez sur **`.gitignore `** pour ouvrir le fichier et le compléter de la manière suivante (sans oublier de le sauvegarder) : 


``` 
   
    # Eclipse
    /.classpath
    /.project
    /.settings/

    # Maven
    /target/
   
```


`# Eclipse` est un commentaire (optionnel). Sont indiqués ensuite les fichiers et le répertoire de configuration d'Eclipse que nous ne souhaitons pas soumettre au gestionnaire de version (`.classpath` , `.project` et `settings/`).

Outre ces fichiers, il est d'usage d'exclure du gestionnaire de version le répertoire `target/` dans le cadre d'un projet Maven qui contient entre autres les classes compilées de notre projet. Ce répertoire peut être reconstitué à partir du code source.

*Remarque :* Ce fichier vise bien sûr à être personnalisé en fonction des données pertinentes à versionner : `.metadata/`, `*.log` ou autres pourront être ajoutés selon vos besoins (en fonction du type de projet, du depôt git, de l'IDE, ... sur lesquels vous travaillez).    
Pour en savoir plus, jetez un rapide petit coup d'oeil à :   
- [La rubrique gitignore dans la documentation de git](https://git-scm.com/docs/gitignore#_pattern_format)  
- [A collection of useful .gitignore templates](https://github.com/github/gitignore)    
- [A .gitignore file for Intellij and Eclipse with Maven](http://gary-rowe.com/agilestack/2012/10/12/a-gitignore-file-for-intellij-and-eclipse-with-maven/)  
  

### 3.  Visualiser et choisir à partir de l'IDE les fichiers à versionner (ou pas ...)

La vue `Navigator` permet de visualiser (et de choisir) les fichiers qui seront soumis au gestionnaire de version.
Des icônes spécifiques au gestionnaire de sources apparaissent pour tous les fichiers du projet (croix, point d'interrogation, signe `+`).  

* Par exemple, vous pouvez d'ores et déjà remarquer que les fichiers mentionnés dans le fichier `.gitignore` ont **une croix** devant leur icône (ces fichiers ne seront pas pistés par le gestionnaire de versions : ils ne seront donc pas soumis au gestionnaire de versions).  

* Le **point d'interrogation** signifie quant à lui que l'on n'a pas encore indiqué si le fichier devait être pisté ou non par le gestionnaire de versions : c'est actuellement le cas pour le fichier `.gitignore` qui dispose d'un point d'interrogation.  

* Pour soumettre ce fichier au gestionnaire de gestions, il est nécessaire de l'**ajouter à l'index**. Pour cela, placez vous dans la vue `Navigator` sur le fichier `.gitignore`, à l'aide d'un clic droit, sélectionner **`Team -> Add to Index `**.
Une fois cette opération effectuée, le point d'interrogation disparaît et un **signe `+`** apparaît devant l'icône du fichier `.gitignore`. 

***Remarque :*** ***HEAD***, ***Index***, ***working tree***  
- **HEAD** est une référence qui **pointe sur le commit** (enregistrement) sur lequel vont se baser les prochaines modifications qui seront effectuées.   
- L'**Index** est une référence qui contient la **liste des fichiers dont Git doit pister les modifications**.  
- Le **working tree** (arbre de travail) est une référence qui contient est la **liste de tous les fichiers présents dans votre dépôt**, qu’ils soient pistés par Git ou pas. Pour savoir quels fichiers ont été modifiés il suffit de regarder les différences entre le contenu des références HEAD et Working tree.

### 4. Effectuer un premier commit  (`Team -> Commit... `)

Pour commiter les modifications apportées au projet, il suffit de revenir sur la vue `Package explorer` de se positionner sur le projet (`testgit` pour nous), puis d'un clic droit de sélectionner **`Team -> Commit... `**


Une fenêtre s'ouvre. Elle permet de configurer le commit :

* Tout d'abord, en saisissant le **message** qui explicitera la teneur du commit. 
Par exemple : `Premier commit`.
* Ensuite, en indiquant l'**auteur** des modifications et la personne qui les a commitées (**committer**) : avec Git, il n'y a pas de commit anonyme (indiquez votre adresse mail).
* Et enfin, en choisissant le(s) fichier(s) à commiter parmi la liste des fichiers proposés (comme il s'agit d'un premier commit : nous choisirons le `pom.xml` et le `.gitignore`)

Validez le commit en appuyant sur `Commit`.

Jetez alors un petit coup d'oeil dans la vue `Package Explorer`, le nom du projet a changé : **`testgit [testgit master]`**, ce qui signifie que notre dépôt **`testgit`** vient d'être commiter sur la branche **`master`** (le commit a donc bien été pris en compte). 

Jetez alors un petit coup d'oeil dans la vue `Package Navigator`, les fichiers `.gitgnore` et `pom.xml` sont désormais précédés d'une icône en forme de **cylindre orange**, ce qui signifie que ces fichiers sont désormais sous contrôle du gestionnaire de version.  


*Remarque : Vous pouvez jetez un petit coup d'oeil [ici](https://github.com/pierreroth64/git-style-guide), c'est un guide de style git qui vous en dira notamment plus sur comment bien nommer vos commit et vos messages de commit.*

### 5. Continuer à versionner ...

Créez dans le paquetage `fr.unilim.iut.testgit` de `/src/main/java` une classe Java que vous appelerez `HelloWorld`.   

#### 5.1 Commiter l'ajout de la nouvelle classe ...
Pour commiter cette classe, il suffit de rappeler **`Team -> Commit `** :  
- en indiquant **`Création de HelloWorld`** comme **message** de commit  
- en chochant le fichier **`HelloWorld.java`** dans la liste des fichiers (pour un ajout dans l'**index**).

... N'oubliez pas vérifier que le **cylindre orange** est bien apparu sur l'icône de la classe après le commit (cette icône est également visible aussi bien depuis la vue `Package Explorer` que depuis la vue `Package Navigator`).

#### 5.2 Coder un peu ...
Ajouter un peu de code dans cette classe :  

```JAVA

	public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
    }
```  

#### 5.3 Commiter les modifications de code ...
Effectuer un nouveau commit à l'aide de  **`Team -> Commit `** :
  
- en indiquant **`ajout main`** comme message de commit.  
*Remarque :* Il est possible d'écrire un message de commit sur plusieurs lignes. Toutefois, il faut savoir que la première ligne est le ***titre*** de votre commit, c'est-à-dire celle qui sera visible lorsque la liste des commits sera affichée (en local ou sur GitHub). Les autres lignes permettent juste de fournir plus de détails sur les changements effectués, si besoin.

- en vérifiant que le fichier **`HelloWorld.java`** est toujours bien coché dans la liste des fichiers à soumettre au gestionnaire de versions (c-a-d des fichiers présents dans l'**index**).



### 6. Visualiser les commits


Placez-vous sur le projet dans la vue `Package Explorer`, puis faites un clic droit afin de sélectionner **`Team -> Show in History `**. La vue `History` s'ouvre. Elle permet de visualiser graphiquement l'ensemble des commits et leur message (ou plutôt le titre du message).



## Connecter un projet Eclipse à Github <a id="eclipse2Github"></a>

### 1. Créer un compte sur Github

Commencez par créer un compte Github si vous n'en avez pas déjà un à partir du lien suivant : [http://github.com/join](http://github.com/join).  
  
En tant qu'étudiant, vous pouvez profiter de l'offre Github Education pour bénéficier de dépots privés et d'autres ressources. Une fois votre compte Github créé, rendez-vous sur [https://education.github.com/](https://education.github.com/) pour profiter de cette offre.


### 2. Créer un dépôt distant sur Github

Créez un nouveau dépôt sur Github en suivant la procédure décrite dans la [section **Create a new repository on GitHub** de la page **Create A Repo**](https://help.github.com/articles/create-a-repo/). Vous pouvez appeler ce dépôt comme vous le souhaitez.  

Pour ce tutoriel, nous utiliserons le même nom que le dépôt local, à savoir : `testgit`.  
Pour cette prise en main, pas besoin de `README`, ni de `.gitgnore`, ou de licence, laissez ces champs à `None` pour le moment.

Copier l'**URL HTTPS** de votre dépôt (https://github.com/username/testgit.git où username est votre pseudo GitHub) dont nous allons nous resservir très vite ...


### 3. Connecter le dépôt distant (Github) au dépôt local (Eclipse)

La connection entre le dépôt distant (appelé **remote**) et le dépôt local va se faire sous Eclipse à l'aide de la vue **`Git Repositories`**.

Ouvrez la vue `Git Repositories` à partir de la barre de menus en haut de l'IDE en sélectionnant **`Window -> Show View -> Other... -> Git -> Git Repositories `**.

La vue `Git Repositories` apparaît alors dans votre IDE, probablement en dessous de la vue `Package Explorer`. Cette vue contient la liste de vos dépôts locaux (repositories). 

Ouvrez le dépôt que vous souhaitez envoyer sur Github (normalement ce devrait être `testgit`).
Placez-vous sur le **`Remotes`** de ce dépôt, puis à l'aide d'un clic droit choisir **`Create Remote...`**. 

Une nouvelle fenêtre `New Remote` apparaît qui permet de configurer le dépôt distant :

- tout d'abord en lui choisissant un **nom**. Par défaut, le nom est **`origin`**. 
Nous laisserons ce nom par défaut car `origin` a une signification particulière dans Git.
C'est sous ce nom que notre repository git apparaîtra dans la vue `Git Repository`.

- ensuite en choisissant entre l'opération `push` et l'opération `fetch`.  
Pour l'instant, nous allons configurer l'opération **`push`** (nous configurerons l'option `fetch` un peu plus tard). Sélectionnez donc **`Configure Push`** puis **`OK`**.   
Une fenêtre **`Configure Push`** s'ouvre :
	- Le premier paramétrage concerne l'**`URI`** qui n'est autre que l'**URL HTTPS** de **votre** dépôt distant Github (https://github.com/username/testgit.git). Cliquez sur **`Change`** pour ouvrir la fenêtre `Select a URI` et collez dans le champ **`URI`** l'adresse de votre dépôt GitHub (https://github.com/username/testgit.git). Dans la zone `Authentification`, n'oubliez pas d'entrer votre nom de compte GitHub (**`User`**) et votre mot de passe (**`Password`**), et de cocher la case **`Store in Secure Store`**. Cliquez sur **`Finish`** pour terminer la configuration de l'`URI`

	- le second paramétrage concerne plus précisément la configuration de l'opération **`push`**. Cliquez sur **`Advanced...`** pour ouvrir la fenêtre `Configure Push`. Nous allons choisir une configuration simple pour commencer qui consiste à ***pousser toutes les branches locales*** (nous n'en avons en fait qu'une seule pour le moment dans ce tutoriel). Pour cela, cliquez sur le bouton **`Add All Branches Spec`**. Nous puvons également choisir de ***pousser tous les tags locaux*** en cliquant sur le bouton ***`Add All Tags Spec`***.
	Une fois ces deux boutons cliqués, deux lignes doivent apparaître dans le tableau `Specifications for push` (ne cochez rien d'autres dans le tableau pour le moment). Terminez simplement la configuration du push en cliquant sur **`Finish`**

Cliquez sur le **`Save`** pour sauvegarder cette nouvelle configuration de `Remotes` (si vous ne voyez pas le bouton `Save`, aggrandissez la fenêtre :smile: ) 

Jetez un petit coup d'oeil dans la vue `Git Repositories` pour vérifier que votre configuration a bien été prise en compte. 
Pour cela, ouvrez le noeud `Remotes` qui contient le **dépôt distant Github avec le nom `origin`** (nom donné lors de la configuration) composé lui-même de **deux sous-noeuds** portant tous deux le nom de  `https://github.com/username/testgit.git`, mais l'un étant précédé d'une petite flêche verte et l'autre d'une petite flêche rouge :   
	- la **flèche rouge** symbolisant l'opération de **`push`** qui permet de pousser notre dépôt local vers Github  
	- la **flèche verte** symbolisant l'opération de **`fetch`** qui permet de rapatrier le contenu du dépôt distant Github vers notre dépôt local Eclipse.  



## Echanger des données entre un dépôt local (Eclipse) et un depôt distant (Github) <a id="interactionEclipseGithub"></a>

Récapitulons, nous disposons actuellement de deux dépôts :  
- un dépôt local dans lequel se trouve notre projet Java `testgit` et sur lequel nous avons déjà éffectué quelques commits.  
- un dépôt distant sur Github. Pour l'instant ce dépôt est vide. 

Ces deux dépôts viennent d'être configurés pour s'échanger des données. Nous allons donc voir maintenant comment :  
- [1. *Pousser* des données du dépôt local d'Eclipse vers le dépôt distant Github via l'opération `push`](#"pushEclipseGithub)  
- [2. *Récupérer* des données du dépôt distant Github vers le dépôt local d'Eclipse via l'opération `fetch`](#fetchGithubEclipse)  


 <img src="http://4.bp.blogspot.com/-EazvwHGDbuE/UTYjC9YEbdI/AAAAAAAAAW0/7o_KEdagB6E/s1600/archi-50.png" width="600">  
 
(Source image : [ici](http://javablabla.blogspot.fr/2013/03/eclipse-egit-github-tuto.html))

### 1. *Pousser* des données du dépôt local d'Eclipse vers le dépôt distant Github via l'opération `push`<a id="pushEclipseGithub"></a>

#### Effectuer un `push ` (ROUGE)
Pour pousser les fichiers de notre dépôt local vers Github nous allons utiliser la **vue `Git Repositories`** et plus particulièrement le **sous-noeud `https://github.com/username/testgit.git` précédé de la flèche ROUGE** présent dans le `origin` du `Remotes` de notre projet.

Placez-vous sur ce noeud, puis à l'aide d'un clic droit sélectionnez **`Push`**.   
Une première fenêtre s'ouvre qui indique la progression de l'opération.  
  
Lorsque l'opération est terminée, une seconde fenêtre `Push Results : testgit-origin` indique que l'opération a bien été effectuée.  
En effet, **`master -> master [new branch]`** signifie que notre branche par défaut (qui s'appelle `master`) a été envoyée vers Github. Vous pouvez alors cliquer sur `OK`.

#### Consulter en ligne les données *poussées*

Rendez-vous via votre navigateur sur votre dépôt Github (https://github.com/username/testgit) pour vérifier que les fichiers soumis au gestionnaire de version ont bien été poussés correctement vers le dépôt distant.  

Pour ce tutoriel, vous devriez donc retrouver dans votre dépôt Github :  `pom.xml`, `.gitignore` et le fichier `HelloWorld.java` dans son package correspondant.

Vous pouvez également constater que votre dépôt a actuellement 3 commits.  
Cliquez d'ailleurs sur `3 commits` pour consulter l'historique de ces commits dans lequel vous retrouvez pour chaque commit le titre du message du commit.


#### Ajouter un fichier README.md
*Remarque : Il est conseillé pour chaque dépôt Github de créer un fichier README.md qui décrit, au [format Markdown](https://fr.wikipedia.org/wiki/Markdown), le contenu de ce dépôt . Le markdown supporté par Github est rappelé [ici](https://guides.github.com/features/mastering-markdown/)*  
  
Nous allons donc maintenant ajouter ce fameux fichier **`README.md`** à notre dépôt Github.  
Pour ce tutoriel, nous procéderons directement à cet ajout en ligne.  
Revenez, via votre navigateur, sur votre dépôt Github (https://github.com/username/testgit).  
Vous devriez apercevoir un bouton vert intitulé **`Add a README`**.    

Cliquez sur ce bouton et complétez le fichier `README.md` via l'onglet `Edit new file` avec le texte suivant par exemple : `# Tutoriel de prise en main de git sous Eclipse`.  
  
Une fois le texte écrit, rendez-vous dans la section du dessous intitulée **`Commit new file`** :    
- la première ligne correspond au **titre du message** du commit que vous renseignerez par exemple avec : **`Ajout README`**.  
- le cadre permet d'indiquer les **détails du message** de commit, comme il peut être optionnel, nous le laisserons vide pour ce commit.  
  
Il ne reste plus qu'à cliquer sur le bouton **`Commit new file`** (en ayant pris soin au préalable de vérifier que la case `Commit directly to the master branch.` était bien cochée).  
Normalement, votre nombre de commits a été incrémenté de 1 (vous deviez être désormais à `4 commits`) et le contenu de votre fichier README apparaît désormais à l'affichage de votre dépôt.


### 2. *Récupérer* des données du dépôt distant Github vers le dépôt local d'Eclipse via l'opération `fetch`<a id="fetchGithubEclipse"></a>

Il est intéressant de pouvoir récupérer les données du dépôt distant lorsqu'une modification a été faite sur ce dépôt. Normalement, si ces modifications proviennent des collaborateurs qui travaillent sur le même projet que vous.

En créant le fichier `README.md`, nous venons de simuler une modification des données présentes sur notre dépôt distant. Pour **récupérer ces modifications sur notre dépôt local**, nous allons utiliser l'**opération `fetch`** via la **vue `Git Repositories`** (tout comme nous venons de procéder pour l'opération `push`). 
Mais avant cela, il est nécessaire de procéder à la configuration de l'opération `fetch`.

#### Configurer l'opération `fetch` (VERT)

Placez-vous dans la **vue `Git Repositories`** sur le **sous-noeud `https://github.com/username/testgit.git` précédé de la flèche VERTE** présent dans le `origin` du `Remotes` de notre projet, puis à l'aide d'un clicdroit sélectionnez **`Configure Fetch...`**


Une fenêtre **`Configure Fetch`** s'ouvre :
  
- le premier paramétrage concerne l'**`URI`** qui n'est autre que l'**URL HTTPS** de **votre** dépôt distant Github (https://github.com/username/testgit.git). Ce paramètre devrait déjà être renseigné puisque nous avons précédemment procéder à la configuration de l'opération `push`.
  
- il ne reste plus qu'à procéder au second paramétrage (**`Ref mappings`**) propre à la configuration de l'opération **`fecth`**. Cliquez sur **`Add`** pour ouvrir la fenêtre **`Adding a RefSpec for Fetch`**.  
Dans le champs **`Remote repository`**, vous devriez trouver l'adresse de votre dépôt distant sur Github.  
Dans le champs **`source`**, aidez-vous de la completion `Ctrl+espace ` pour choisir la branche **`master`**.  
Cliquez sur **`Next`**, **`Finish`** puis **`Save`**.

*Remarque : Comme pour le `push`, cette opération ne configuration n'aura besoin d'être effectuée qu'une seule fois.*
	

#### Effectuer un `fetch` (rappatrier les données distantes vers le dépôt local)...

Placez-vous dans la **vue `Git Repositories`** sur le **sous-noeud `https://github.com/username/testgit.git` précédé de la flèche VERTE** présent dans le `origin` du `Remotes` de notre projet, puis à l'aide d'un clic droit sélectionnez **`Fetch`**

Une première fenêtre s'ouvre qui indique la progression de l'opération.  
Lorsque l'opération est terminée, une seconde fenêtre `Fetch Results : testgit-origin` indique **`master : origin/master [new branch]`** qui signifie que le données ont bien été rappatriées du dépôt distant vers notre dépôt local. Vous pouvez cliquer sur `OK`.


#### Consulter en local les données *rapatriées*

En consultant le projet `testgit` dans la vue `Package Explorer` ou `Navigator`, on constate que le fichier `README.md` n'apparaît pas dans notre projet, pourtant il a bien été rappatrié (téléchargé).

Ouvrez la **vue `History`** (`Team -> Show in History `).  
Cliquez sur le bouton **`Show all branches and tags`** de cette vue (ce bouton est le dernier des boutons en haut à droite de la vue).

Cette vue nous indique qu'un commit a bien été ajouté à notre dépôt (**`origin/master Ajout README`**).  
Toutefois, le commit sur lequel nous nous trouvons est le commit en surlignance c-a-d que rien n'a pas pour l'instant changé dans notre espace de travail. 
Il est donc nécessaire maintenant de mettre à jour notre espace de travail avec ce nouveau commit c-a-d de fusionner les données distantes rappatriées sur le dépôt local avec les données actuellement présentes sur le dépôt local. Cette opération s'appelle `merge` sous git.


#### *Merger* les données distantes rappatriées aux données présentes sur le dépôt local (mettre à jour de l'espace de travail)  

Sélectionnez dans la **vue `History`** le commit sur lequel vous souhaitez travailler c-a-d celui que vous souhaitez voir apparaître dans votre espace de travail (dans votre vue `Package Explorer`). Pour cette partie du tutoriel, nous souhaitons bien évidemment travailler sur le **dernier commit**, celui vient du dépôt distant et qui correspond à **`origin/master Ajout README`**.  

A l'aide d'un **clic droit**, sélectionnez **`Merge`** depuis ce commit. Une fenêtre `Merge Results` s'ouvre et donne des informations sur le merge. Cliquez sur **`OK`** pour réaliser l'opération `merge`.

Jetez maintenant un petit coup d'oeil au contenu de votre projet `testgit` via la vue `Package Explorer`. Le fichier `README.md` est bien présent dans ce projet. Le commit sur lequel nous travaillons désormais est bien le dernier commit de notre dépôt.


## Importer dans Eclipse un projet hébergé sur Github <a id="Github2Eclipse"></a>

Dans cette partie du tutoriel, nous allons importer localement le code proposé sur le dépôt distant Github suivant : [https://github.com/IUT-Info-Limoges/basicloop](https://github.com/IUT-Info-Limoges/basicloop). Ce code nous servira peut-être un jour, qui sait ? ...

Pour importer directement le contenu d'un dépôt distant dans Eclipse, il suffit de sélectionner **`Import... -> Git -> Projects from Git`** soit directement à partir d'un **clic droit dans la vue `Package Explorer`**, soit en passant par le menu **`File`** dans la barre de menus en haut de l'IDE.  

Une fois **`Projects from Git`** sélectionne, cliquez sur **`Next`** pour continuer.  
Choisissez alors **`Clone URI`** puisque le projet à importer se trouve sur un dépôt distant et terminez par `Next`.  

Une fenêtre **`Source Git Repository`** s'ouvre.  
Renseignez le champ **`URI`** avec l'adresse suivante : **`https://github.com/IUT-Info-Limoges/basicloop.git`**  
Puis dans la zone **Authentification**, renseignez les champs **`User`** (votre nom de compte GitHub) et **`Password`**, cochez la case **`Store in Secure Store`** et cliquez sur `Next`. 

EGit se connecte alors au dépôt distant, récupére la liste des branches et les affiche dans une nouvelle fenêtre **`Branch Sélection`**.  
Sélectionnez la(les) branche(s) que vous voulez voir apparaître dans votre dépôt local.   
Pour ce tutoriel, il suffit de cocher **`master`** (ce qui doit déjà être le cas par défaut), puis de cliquer sur **`Next`**.  

Une fenêtre **`Local Destination`** s'ouvre pour choisir le dossier dans lequel vous voulez que EGit clone ce dépôt.    
Par défaut un réperoire vous est proposé.    
Si vous souhaitez importer ce projet dans votre espace de travail actuel (workspace actuel), il suffit dans le champ **`Directory`** de sélectionner à l'aide du bouton **`Browse`** le chemin vers votre workspace Eclipse. Le nom du projet `basicloop` se rajoute ensuite à ce chemin, ce qui devrait vous donner au final un champ `Directory` de la forme : `cheminDuWorkspaceEclipseActuel\basicloop`. Faîtes en sorte que le chemin de votre workspace actuel apparaisse puis cliquez sur **`Next`**.

Eclipse va créer le dossier à l'emplacement indiqué et y copier le contenu de la branche sélectionnée (vous pouvez le vérifier en ouvrant un explorateur de fichiers).  
Une dernière fenêtre **`Select a wizard to use for importing projects`** demande enfin comment importer le(s) projet(s) depuis notre dépôt cloné. Pour cela, elle propose plusieurs options :  
  
* **Import existing projects** qui demande à Eclipse de lister les projets trouvés dans le dépôt que nous venons de cloner. Pour que des projets soient détectés il faut qu'ils soient valides du point de vue d'Eclipse (fichier `.project` présent dans le dossier racine de chaque projet). Si vous faites `Next` vous verrez que ce n'est malheureusement pas le cas pour ce dépôt. Revenir donc à la fenêtre précédent avec un `Back`.
 
* **Use the New Project wizard** qui demande à Eclipse de créer un nouveau projet à partir des fichiers sources trouvés dans le dépôt. Cette option est utile, si comme dans notre cas, vous disposez d'un dossier `src` avec vos fichiers sources, mais pas de fichier `.project`. Nous préférerons toutefois utiliser la troisième solution. 

* **Import as a general project** qui demande à Eclipse d'importer le projet sans chercher à savoir si c'est un projet Java, PHP ou autre et donc sans configurer certains fichiers annexes comme le classpath, par exemple. Une fois cette option cochée, cliquez sur **`Next`**, puis sur **`Finish`** (si le nom du projet vous convient).

Le projet **`basicloop`** apparaît alors dans la vue **`Package Explorer`** avec son dossier **`src`**, le **`pom.xml`** et le **`README.md`**.  

D'après l'option précédente choisie, ce projet est pour l'instant un projet general.  
Pour le convertir en projet maven, il suffit de vous placer sur le nom du projet `basicloop` dans la **vue `Package Explorer`**, puis à l'aide d'un clic droit de sélectionner **`Configure -> Convert to Maven Project**`... Et hop ! le tour est joué !

## Un petit exercice bien branché qui va fusionner <a id="exercice"></a>

Dans ce petit exercice, nous allons manipuler des branches.  

Revenez sur le projet `testgit`.  
Pour l'instant, nous sommes sur la **branche `master`** de notre projet comme l'indique le nom du projet **`testgit [tesgit master]`**.  

A l'aide d'un clic droit sélectionnez `Shown In -> History` pour ouvrir la vue `History` et retrouver l'historique des 4 commits déjà réalisés sur ce projet.

Nous allons commencer par réaliser un nouveau commit sur `master`.  
Pour cela, ajoutez à la méthode `main` de la classe la `HelloWorld` l'instruction suivante :  
 **`System.out.println( "Jouons ensemble!" );`** juste en dessous de l'instruction `System.out.println( "Hello World!" );` déjà existante.  

Sauvegarder le fichier `HelloWorld.java`, puis committez cette nouvelle modification (via `Team->Commit`) et vérifiez dans la vue `History` que votre commit a bien été pris en compte ! 
   
*Remarque : Dans la vraie vie, on ne committe bien évidémment pas à chaque nouvelle lignes de code, ceci n'est qu'un simple exercice. Il faut toutefois essayer de commiter le plus souvent possible et de faire en sorte que ces commits soient atomiques (un développeur peut committer facilement 10 à 30 fois par jour en local, il pourra ensuite nettoyer son historique avant de pusher...mais c'est une autre histoire que vous pourrez lire un peu plus tard [ici](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/) dans la rubrique "Nettoyer son historique local avant envoi" quand vous aurez fini ce tutoriel par exemple...*



Nous allons commencer par jouer au [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz), un jeu qui consiste à énoncer les nombres de un par un et à remplacer un nombre par :    
* le mot **Fizz** s'il est **multiple de 3**  
* le mot **Buzz** s'il est **multiple de 5**  
* le mot **FizzBuzz** s'il est **multiple de 3 et de 5** 

Pour cela, nous allons créer une branche. Les branches permettent de faire des expérimentations sur votre code c-a-d de développer de nouvelles fonctionnalités sans modifier le code *normalement* opérationnel sur `master`. 

### 1. Créer une branche (`branch` & `checkout`)

Depuis la vue **`Package Explorer`**, positionnez-vous sur le projet `testgit` puis via un clic droit **`Team -> Switch to -> New Branch...`**  
  
Une fenêtre `Create Branch` apparaît. 
Il faut commencer par **sélectionner dans `Source`, la branche à partir de laquelle on souhaite créer** la nouvelle branche : dans notre cas, ce sera **`master`** qui est déjà indiqué par défaut.  
Ensuite, il suffit de saisir le **nom de la nouvelle branche** : **`fizzbuzz`** par exemple.
Laissez **`Checkout new branch`** coché : cela vous permettra de vous retrouver automatiquement sur la nouvelle branche créé une fois le bouton **`Finish`** cliqué.

Pour vous en convaincre, jetez un petit coup d'oeil sur le nom du projet qui apparaît désormais de la sort **`testgit [testgit fizzbuzz]`** indiquant ainsi que l'on travaille désormais bien sur la branche **fizzbuzz**.

Jetez également un petit coup d'oeil à **la vue `History`**.   
**fizzbuzz** apparaît en gras sur le dernier commit, c'est la branche sur laquelle on se situe.
Cette vue nous indique aussi que les branches **`fizzbuzz`** et **`master`** pointent vers le même commit (en l'occurrence le dernier commit) puisque les étiquettes **fizzbuzz** et **master** sont sur la même ligne.  
*Remarque : Il est possible que vous ayez besoin de cliquer sur le bouton **Show all branches**(en haut à droite de cette vue `History`) pour visualiser tous les noms de branches dans la vue `History`*


### 2. Coder dans la nouvelle branche `fizzbuzz`

Le but de cette branche est de développer la nouvelle fonctionnalité : un jeu de **FizzBuzz**.

Commencez par modifier le code de la **classe `HelloWorld`** pour ajouter un appel à `FizzBuzz` de la manière suivante :

``` JAVA       

    public class HelloWorld {
	
	    public static void main( String[] args )
            {
 			System.out.println( "Hello World!" );
    		System.out.println( "Jouons ensemble!" );  
			
			System.out.println( "Et si on jouait au FizzBuzz ?" );  
            System.out.println(FizzBuzz.getResult());
        }
    }  
  
```  

Nous n'allons pas coder le jeu FizzBuzz pour ce tutoriel (un bon exercice serait bien sûr de le coder en TDD), mais nous renverrons directement les valeurs attendues *en dur*.  
Dans le paquetage `fr.unilim.iut` de `src/main/java`, créez donc la **classe `FizzBuzz`** le plus simplement possible de la manière suivante :

``` JAVA  
 
    public class FizzBuzz {

	    public static String getResult() {
		    return "1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, Fizz Buzz";
	    }
    }  

```



Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Ajout FizzBuzz de 1 à 15`**.  
**Ne pas oubliez** de cocher dans **Files** le nouveau fichier créé **`FizzBuzz.java`** pour l'ajouter à l'**`index`**.

Une fois le commit effectué, vérifiez que la classe FizzBuzz a bien une **icône orange**.    Consultez ensuite la **vue `History` pour constater** que ce dernier commit a bien eu lieu sur la branche **`fizzbuzz`** : seule l'étiquette **fizzbuzz** est associée à ce dernier commit (cette vue nous indique donc bien que les branches **`fizbuzz`** et **`master`** ne pointent plus désormais vers le même commit).

### 3. Se déplacer d'une branche à l'autre (`checkout`)

Pour vous déplacer d'une branche à l'autre, il vous suffit de vous positionner sur le projet puis **`Team-> Switch To`** et choisir le nom de la branche.  

En laissant le code de la classe **`HelloWorld`** affiché à l'écran, naviguer entre `master` et `fizzbuzz` avec des **`Team-> Switch To->master`** et des **`Team-> Switch To->master`** afin de bien constater que le contenu de la classe `HelloWorld` des deux classes est bien différent.

Pour continuer le tutoriel, positionnez-vous sur la branche **`master`**.  
Vous pouvez vous assurer que vous êtes sur la branche **`master`** en regardant le nom du projet qui doit être de la forme **`testgit [testgit master]`**


### 4. Comparer le contenu de deux branches (`diff`)

Vous vous trouvez donc actuellement sur la branche **`master`**.
Positionnez-vous dans le fichier `HelloWorld.java` qui contient uniquement deux lignes.

Il est possible de comparer les **diff**érences de ce fichier entre les deux branches.  
Pour cela, depuis le fichier `HelloWorld.java`, à l'aide d'un clic droit sélectionnez **`Compare with -> Branch, Tag or Reference`**, puis sélectionnez la branche à comparer (`fizbuzz` en dessous de `Local`) et cliquez sur **`Compare`**. 

Les deux fichiers `HelloWorld.java` s'affiche côte à côte et les différences entre ces fichiers apparaissent en bleu.
  
Fermez cette comparaison en cliquant sur la croix de l'onglet `Compare HelloWorld.java Current and xxxxxxx` (où xxxxxxx est le numéro de commit que vous retrouvez dans la vue `History`).


### 5. Fusionner la branche `fizzbuzz` dans `master` (`merge` sans conflit) <a id="fusionFastForward"></a>
Pour que la branche `master` incorpore le travail de la branche `fizzbuzz`, nous allons utiliser l'opération **`merge`** de git accessible sous Eclipse via le menu **`Team-> merge`**.

Pour ***merger*** la branche `fizzbuzz` dans la branche `master`, il faut procéder de la manière suivante :  
* placez-vous sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)   
* sélectionnez l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionnez la **branche à fusionner** dans `master` : dans notre cas, c'est **`fizzbuzz`** qui doit être sélectionné.  
* conservez les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Une fenêtre `Merge Result` s'ouvre indiquant que la fusion s'est bien passée en indiquant que le résultat est bien **`Fast-forward`**. Il ne reste plus qu'à cliquer sur **`OK`**.

Remarque : Les résultats possibles lors de la fusion peuvent être de type `Already-up-to-date` (si on essayait par exemple de refusionner une deuxième fois la branche `fizzbuzz` dans `master`), `Fast-forward`, `Merged`, `Conflicting` ou encore `Failed`.


**Remarque (notions avancées) :  **  

* En conservant choisissant l'option par défaut `fast-forward`, git va seulement déplacer l’étiquette de la branche master sur le même commit que fizzbuzz en créant ou non un nouveau commit (c'est ce que vous pouvez constater en consultant la vue History). Le graphe n'isole plus le point de départ de la branche fizzbuzz, et une fois la branche elle-même supprimée (son étiquette de branche), il ne restera plus trace des limites de celle-ci dans le graphe. Cette solution marche bien si la branche et purement locale et temporaire et si le master n'a pas subi de modification depuis la création de la branche (auquel cas un rebase peut être nécessaire).
* Si la branche doit rester identifiable à terme dans l'historique des commit, c-a-d si on souhaite qu'elle reste visible dans le graphe par une « bosse », il faudra exécuter un true merge, c-a-d un véritable commit de fusion (il faut alors choisir l'option no fast-forward). Cette option sera intéressante si la branche a une sémantique claire et documentée et qu’elle doit continuer à apparaître clairement dans le graphe de l'historique.  


Pour en savoir plus, vous pouvez consulter quelques graphiques [ici](https://fr.atlassian.com/git/tutorials/using-branches/git-merge/) et lire tranquillement cet article [là](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/).


### 6. Ajouter un tag sur une branche (`tag`)

Il est possible de tagger un commit.
Pour cela, placez-vous dans la vue `History` sur le commit que vous souhaitez tagger, pour nous ce sera celui du **master**.  
Depuis ce commit, choisissez **`Create Tag`** à l'aide d'un clic droit.  
Dans **`Tag Name`**, donner le nom du tag par exemple **`v1.0`** (pour un numéro de version).  
Vous pouvez aussi renseigner la partie **`Tag Message`** par exemple avec `Mise en place du FizzBuzz`, puis cliquer sur **`Create Tag`**.

Dans la **vue `History`**, une étiquette avec le nom du tag (ici **`v1.0`**) apparaît en tout début du commit taggé (avant les étiquettes contenant le(s) nom(s) de branche).


### 7. Simuler un workflow ...

#### 7.1 Créer une nouvelle branche `marabout` depuis `master`

Positionnez-vous sur **`master`** (via un `Team -> Switch to` si nécessaire). 
Depuis la **vue `Package Explorer`**, clic droit **`Team -> Switch to -> New Branch...`** : 

* dans **`Source`**, sélectionner la **branche à partir de laquelle** on souhaite créer la nouvelle branche (**`master`**)  
* puis saisir le **nom de la nouvelle branche** : **`marabout`**   
* laisser **`Checkout new branch`** pour se retrouver sur cette nouvelle branche une fois **`Finish`** cliqué.

Le nom du projet devient : **`testgit [testgit marabout]`**  
La création de la branche se voie aussi sur la vue `History` via une nouvelle étiquette **`marabout`** sur le dernier commit.

#### 7.2 Coder et commiter dans la nouvelle branche `marabout`

Le but de cette branche est de développer la nouvelle fonctionnalite de type **jeu de Marabout**.

>[Le marabout est un jeu d'esprit. Il s'agit, à partir d'une expression initiale, de construire une suite d'expressions ou une suite de mots dont les premières syllabes correspondent phonétiquement aux dernières de l'expression précédente.](https://fr.wikipedia.org/wiki/Marabout_(jeu)) (extrait Wikipédia)


##### Coder, committer ...
Modifiez le code de la **classe `HelloWorld`** pour ajouter un appel à `Marabout` de la manière suivante :

``` JAVA       

    public class HelloWorld {
	
	    public static void main( String[] args )
            {
 			System.out.println( "Bonjour tout le monde!" );  
    		System.out.println( "Jouons ensemble!" );  
			
			System.out.println( "Et si on jouait au FizzBuzz ?" );  
            System.out.println(FizzBuzz.getResult());

			System.out.println( "Et si on jouait au Marabout ?" );  
            System.out.println(Marabout.getResult());
        }
    }  
  
```  



Comme pour le jeu FizzBuzz, nous renverrons directement *en dur* les valeurs attendues. 

Dans le paquetage `fr.unilim.iut` de `src/main/java`, créez la **classe `Marabout`** le plus simplement possible de la manière suivante :

``` JAVA  
 
    public class Marabout {

	    public static String getResult() {

		    return "Trois p'tits chats, trois p'tits chats, trois p'tits chats, chats, chats, \n"
				   + "Chapeau d'paille, chapeau d'paille, chapeau d'paille, paille, paille, \n"
				   + "Paillasson, paillasson, paillasson, -son, -son,";
	     }
} 

```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Ajout Marabout 3 phrases`** et en veillant bien à cocher dans **Files** le nouveau fichier créé **`Marabout.java`** pour l'ajouter à l'**`index`**.

Une fois le commit effectué, vérifiez que la classe **`Marabout`** a bien une **icône orange**.   
Consultez la **vue `History` pour constater** que ce dernier commit a bien eu lieu sur la branche **`marabout`**.  


##### Coder, committer ...

Modifiez le code de la **classe `Marabout`** en ajoutant quelques lignes de code suppléméntaires :

``` JAVA  
 
    public class Marabout {

	    public static String getResult() {

		    return "Trois p'tits chats, trois p'tits chats, trois p'tits chats, chats, chats, \n"
				+ "Chapeau d'paille, chapeau d'paille, chapeau d'paille, paille, paille, \n"
				+ "Paillasson, paillasson, paillasson, -son, -son, \n"
				+ "Somnambule, somnambule, somnambule, -bule, -bule, \n"
				+ "Bulletin, bulletin, bulletin, -tin, -tin, \n"
				+ "Tintamarre, tintamarre, tintamarre, -marre, -marre,";
	     }
} 

```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Suite Marabout avec 3 phrases de plus`**  
(en vérifiant que le fichier modifié à savoir **`Marabout.java`** est bien coché dans  **Files**).

#### 7.3 Coder et commiter dans la branche `fizzbuzz`

Revenez sur la branche **`fizzbuzz`** (**`Team -> Switch To -> fizzbuzz`**).
Vous pouvez constater que la classe **`Marabout`** n'existe pas dans la branche **`fizzbuzz`**, c'est normal puisque elle a été créée dans la branche **`marabout`**.

##### Coder, committer ...
La branche **`fizzbuzz`** nous permet de développer la fonctionnalité **FizzBuzz**.
Ecrivons donc quelques lignes de code supplémentaires dans la classe **`FizzBuzz`** :

``` JAVA  
 
    public class FizzBuzz {

	    public static String getResult() {
		    return "1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, Fizz Buzz"
				    + ", 16, 17, Fizz, 19, Buzz, Fizz, 22, 23, Fizz, Buzz, 26, Fizz, 28, 29, Fizz Buzz";

	    }
    }  

```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Suite FizzBuzz de 16 à 30`**  
(en vérifiant que le fichier modifié à savoir **`FizzBuzz.java`** est bien coché dans **Files**).


##### Coder, committer ...
Et codons encore un peu pour procéder à un nouveau commit.
Complétez la classe **`FizzBuzz`** de la manière suivante :

``` JAVA  
 
    public class FizzBuzz {

	    public static String getResult() {
		    return "1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, Fizz Buzz"
				    + ", 16, 17, Fizz, 19, Buzz, Fizz, 22, 23, Fizz, Buzz, 26, Fizz, 28, 29, Fizz Buzz"
				    + ", 31, 32, Fizz, 34, Buzz, Fizz, 37, 38, Fizz, Buzz, 41, Fizz, 43, 44, Fizz Buzz";

	    }
    }  

```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Suite FizzBuzz de 31 à 45`**  
(en vérifiant que le fichier modifié à savoir **`FizzBuzz.java`** est bien coché dans **Files**).

Jetez un petit coup d'oeil dans la **vue `History`**.  
Vous devriez voie apparaître une distinction entre les branches **`marabout`** et **`fizzbuzz`**.  
Si tel n'est pas le cas, cliquez sur le bouton **Show all branches** (en haut à droite de la vue `History`) pour pouvoir visualiser les différentes branches.


#### 7.4 Fusionner `fizzbuzz` dans `master` (*fast-foward*)

Pour ***merger*** la branche `fizzbuzz` dans la branche `master`, procédez comme précédemment :  

* placez-vous sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)     
* sélectionnez l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionnez la **branche à fusionner** dans `master` : **`fizzbuzz`**   
* conservez les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Une fenêtre `Merge Result` s'ouvre indiquant que la fusion s'est bien passée en indiquant que le résultat est bien **`Fast-forward`**. Il ne reste plus qu'à cliquer sur **`OK`**.


#### 7.5 Fusionner `marabout` dans un `master` modifié depuis la création de la branche ... (*true merge*)


Pour ***merger*** la branche `marabout` dans la branche `master`, procédez de la même manière :  
  
* se placer sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)     
* sélectionner l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionner la **branche à fusionner** dans `master` : **`marabout`**   
* conservez les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Une fenêtre `Merge Result` s'ouvre indiquant que la fusion s'est bien passée en indiquant que le résultat est cette fois-ci **`Merge`**. Il ne reste plus qu'à cliquer sur **`OK`**.

Cette fois-ci la fusion ne s'est pas faite en *fast-forward*, mais c'est une *vraie* fusion (un  **true merge**) qui a été réalisée puisque la branche **master** avait évoluée depuis la création de la branche **marabout** (en l'occurrence avec deux commitx supplémentaires créés par la fusion *fast-forward* de **fizzbuzz**). 


**Remarques (notions avancées) :**

* Jetez un petit coup d'oeil à la  **vue `History`**, elle va nous permettre d'illutrer la remarque (notions avancées) que nous avions fait précédemment dans la partie *[5. Fusionner la branche `fizzbuzz` dans `master` (`merge` sans conflit)](#fusionFastForward)* de ce tutoriel. En effet, la **vue `History`** nous permet de visualiser que : 

	* le *fast-forward* de la fusion de la branche **fizzbuzz** dans **master** a juste déplacé l'étiquette de **master** sur le dernier commit et **intégré** les commits de la branche **fizzbuzz** dans la branche **master**, ce qui donne un **historique linéaire** et qui est tout l'intérêt du *fast-forward*.

	* la vraie fusion (*true merge*) de la branche **marabout** a, quant à elle, donné lieu à un nouveau commit dans la **vue `History`** appelé par Egit **`Merge branch marabout`**. Cette *vraie* fusion permet à la branche dans le graphe par une « bosse » et donc de continuer à **apparaître clairement dans l'historique**.

Remarque : On se retrouve en fait dans la configuration suivante :

![Historique FastForward(fizzbuzz) Merge(marabout)](images/Historique_FastForward_Merge.png)


* Lorsque le **master** a évolué (comme dans le cas de la fusion de **marabout**), il est toutefois possible de faire en sorte que l'historique reste linéaire si au lieu d'effectuer directement un **true merge**, on effectue à la base un **rebase** suivi d'un **fast-forward**. Alors **rebase/fast-forward vs rebase** (*historique linéaire ou non?*) : des discussions sur le sujet peuvent être consultées [ici](http://www.git-attitude.fr/2014/05/04/bien-utiliser-git-merge-et-rebase/), [là](https://fr.atlassian.com/git/tutorials/merging-vs-rebasing) ou encore [là](http://webadeo.github.io/git-simpler-better-faster-stronger/#1.0) : un vrai débat existe dans lequel nous n'entrerons pas dans le cas de ce tutoriel ...


<!-- Remarque : Le nom de votre projet doit être de la forme **`testgit [testgit master]`**. Si jamais, il était de la forme **`testgit [testgit Merged master]`**, ce qui peut être le cas quand la fusion de de 2 branches créée un commit à plusieurs parents si les branches ont divergé, faites un nouveau commit derrière la fusion...-->


#### 7.6 Coder et commiter dans une nouvelle branche `marabout-simple`

Le but de cette branche est de développer une nouvelle fonctionnalite de type **jeu de Marabout**, mais plus simple que la précédente !

##### Créer une nouvelle branche `marabout-simple`

Positionnez-vous sur **`master`** (via un `Team -> Switch to` si nécessaire).  
Depuis la **vue `Package Explorer`**, clic droit **`Team -> Switch to -> New Branch...`** : 

* dans **`Source`**, veillez bien à vous trouver sur **`master`** : la **branche à partir de laquelle** on souhaite créer la nouvelle branche.     
* saisir le **nom de la nouvelle branche** : **`marabout-simple`**   
* laisser **`Checkout new branch`** 

Le nom du projet devient : **`testgit [testgit marabout-simple]`**  
Un petit coup d'oeil à la **vue `History`** permet d'y apercevoir l'étiquette **marabout-simple** sur le dernier commit.

##### Coder, committer ...
Modifiez le code de la **classe `HelloWorld`** pour ajouter un appel à `MaraboutSimple` de la manière suivante :

``` JAVA       

    public class HelloWorld {
	
	    public static void main( String[] args )
            {
 			System.out.println( "Bonjour tout le monde!" );  
    		System.out.println( "Jouons ensemble!" );  
			
			System.out.println( "Et si on jouait au FizzBuzz ?" );  
            System.out.println(FizzBuzz.getResult());

			System.out.println( "Et si on jouait au Marabout ?" );  
            System.out.println(Marabout.getResult());

			System.out.println( "Et si on jouait au Marabout de manière plus simple ?" );  
            System.out.println(MaraboutSimple.getResult());
        }
    }  
  
```  

Dans le paquetage `fr.unilim.iut` de `src/main/java`, créez la **classe `MaraboutSimple`** le plus simplement possible de la manière suivante :

``` JAVA  
 
    public class MaraboutSimple {
	
	    public static String getResult() {
           return "Marabout, Bout de ficelle, Selle de cheval";
     }

   }   
```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Ajout Marabout simple 3 mots`** et en veillant bien à cocher dans **Files** le nouveau fichier créé **`MaraboutSimple.java`** pour l'ajouter à l'**`index`**.

Une fois le commit effectué, vérifiez que la classe **`MaraboutSimple`** a bien une **icône orange** et consultez la **vue `History`** pour visualiser ce dernier commit et l'apparition dans l'historique d'une étique **marabout-simple**. 


##### Coder, committer ...

Modifiez le code de la **classe `MaraboutSimple`** en ajoutant quelques lignes de code suppléméntaires :


``` JAVA  
 
    public class MaraboutSimple {
	
	    public static String getResult() {
           return "Marabout, Bout de ficelle, Selle de cheval"
	              + ", Cheval de course, Course à pied, Pied à terre";
     }

   }   
```

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

**Committez (`Team->Commit`)** avec un message du genre **`Ajout Marabout simple 3 mots de plus`** et en veillant bien à ce que le fichier **`MaraboutSimple.java`** soit coché.  
Consultez la **vue `History`** pour visualiser ce dernier commit.

**Volontairement, on ne fusionnera pas cette branche tout de suite dans master !!!**  
*... car nous souhaitons simuler le fait qu'en parallèle du développement de la fonctionnalité **MaraboutSimple**, une autre tâche de développement a été menée dans une autre branche ... et pour les besoins de ce tutoriel, ce sera un peu **refactoring** de code...*


#### 7.7 Refactorer un petit peu le code dans une branche `refactoring`

Tiens, et si on refactorait (remaniait) un peu notre code... Pour cela, nous allons créé une branche dédiée au refactoring...

##### Créer une nouvelle branche `refactoring` depuis `master`

Positionnez-vous sur **`master`** (via un `Team -> Switch to` si nécessaire).  
Depuis la **vue `Package Explorer`**, clic droit **`Team -> Switch to -> New Branch...`** : 

* dans **`Source`**, veillez bien à vous trouver sur **`master`** : la **branche à partir de laquelle** on souhaite créer la nouvelle branche.     
* saisir le **nom de la nouvelle branche** : **`refactoring`**   
* laisser **`Checkout new branch`** 

Le nom du projet devient : **`testgit [testgit refactoring]`**  
Un petit coup d'oeil à la **vue `History`** permet d'y apercevoir l'étiquette **refactoring** sur le dernier commit.


##### Modifier le code et committer ...

Le refactoring que nous souhaitons mettre en place concerne le nommage du code afin que ce dernier montre mieux son intention. En effet `getResult` n'est pas très parlant comme nom de méthode, nous pourrions par exemple le remplacer par `jouer` ou `enoncer` pour mieux refléter l'intention.

Dans la classe **`HelloWorld`**, remplacez les appels à `getResult()` par des appels à `jouer()`.  
Dans les classes **`Fizzbuzz`** et **`Marabout`**, renommez les `getResult` en `jouer`.
Remarque : la classe **`MaraboutSimple`** n'apparaît pas pour le moment, c'est normal car elle est en train d'être développée dans une autre branche qui n'a pas encore été fusionnée avec **master**... Contentez-vous donc de procéder aux modifications dans les classes existantes sur **master** : **`Fizzbuzz`** et **`Marabout`**.

Remarque : Vous pouvez faire ce refactoring facilement à partir de votre IDE :  
-> placez-vous dans la classe  **`HelloWorld`** sur le `getResult` de l'instruction `FizzBuzz.getResult()` puis d"un clic droit : **Refactor...->Rename**, puis saisir `jouer` : la modification sera automatiquement pris en compte pardans la classe FizzBuzz après avoir valider par un appui sur **Entrée**.  
-> faites de même, et rempalcez le `getResult` de l'instruction `Marabout.getResult()` par un `jouer`.


Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !


**Committez (`Team->Commit`)** avec un message du genre **`refactoring nommage`** et en veillant bien à ce que les 3 fichiers **`HelloWorld.java`**, **`Fizzbuzz.java`** et **`Marabout.java`** apparaissent bien dans **Files**.

Une fois le commit effectué, vérifiez que la classe **`Marabout`** a bien une **icône orange**.   
Consultez la **vue `History` pour constater** que ce dernier commit a bien eu lieu sur la branche **`marabout`**.  


Actuellement, deux branches sont tirées depuis **master** : il s'agit de **marabout-simple** et **refactoring** (comme le montre l'historique suivant ). 

![Historique_Refactoring_MaraboutSimple_AvantFusion](images/Historique_Refactoring_MaraboutSimple_AvantFusion.png)

Ces branches ont donnée lieu respectivement à deux et un commit... Il ne reste plus qu'à fusionner ces modifications dans **master**, et pour cela nous commencerons par la branche **refactoring**.

#### 7.8 Fusionner `refactoring` dans un `master` (*fast-forward*)

Pour ***fusionner*** la branche `refactoring` dans la branche `master`, procédez classiquement :  
  
* se placer sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)     
* sélectionner l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionner la **branche à fusionner** dans `master` : **`refactoring`**   
* conservez les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Une fenêtre `Merge Result` s'ouvre indiquant que la fusion s'est bien passée en indiquant que le résultat est cette fois-ci **`Fast-forward`**. Il ne reste plus qu'à cliquer sur **`OK`**.

La branche **refactoring** a donc pu être intégré à la branche **master** en *fast-forward* puisque **master** n'avait pas subi de modication depuis que la branche refactoring avait été tirée. Les étiquettes **master** et **refactoring** pointent sur le même commit (le dernier) comme le montre la **vue `History`** ou l'image ci-après :
![Historique_Refactoring_FastForward](images/Historique_Refactoring_FastForward.png)


#### 7.9 Fusionner `marabout-simple` dans un `master` (résoudre un conflit !)

Pour ***fusionner*** la branche `marabout-simple` dans la branche `master`, procédez classiquement :  
  
* se placer sur la branche qui doit recevoir la fusion : `master` (via un `Team -> Switch to` si nécessaire)     
* sélectionner l'option de fusion via un clic droit et **`Team -> merge`**    
* sélectionner la **branche à fusionner** dans `master` : **`marabout-simple`**   
* conservez les options par défaut (`commit` et `fast-forward`) et cliquez sur **`Merge`**.

Et là...Aie!!!Aie!! La fenêtre `Merge Result` s'ouvre en indiquant que la fusion a donné lieu a un **conflit** : **`Result Conflicting`** : c'est-à-dire que git n'a pas réussi à faire la fusion tout seul et qu'il va falloir un peu l'aider pour résoudre ce conflit et permettre la fusion... Il ne reste plus qu'à cliquer sur **`OK`**.

Le nom du projet est actuellment **`testgit [testgit|Conflicts master]`** : nous sommes donc sur une branche *temporaire* où nous allons devoir intervenir dans le code pour résoudre ce conflit...

Dans la **vue `Package Explorer`**, les conflits sont indiqués par des petites icônes rouges.

Le fichier dans lequel se situe le conflit est d'ailleurs affiché à l'écran (pour nous ce sera `HelloWorld.java`).
Le conflit est repéré dans ce fichier par des marqueurs.

```Java  

    <<<<<<< HEAD
        System.out.println(Marabout.jouer());
    =======
        System.out.println(Marabout.getResult());
        
        System.out.println( "Et si on jouait au Marabout de manière plus simple ?" );  
        System.out.println(MaraboutSimple.getResult());
    >>>>>>> refs/heads/marabout-simple
```	

Ici, egit n'a pas réussi à fusionner le code entre les marqueurs  `<<<<<<<` et  `>>>>>>>`.
Dans une branche, il se trouvait avec le code au dessus du ` =======` et dans l'autre avec le code en-dessous. 
Remarque : Si vous souhaitez en savoir plus sur les marqueursn c'est par [ici](https://www.kernel.org/pub/software/scm/git/docs/git-merge.html#_how_conflicts_are_presented).


Que faire ? C'est maintenant à nous de décider comment fusionner...  


Il est possible de visualiser l'origine du conflit en utilisant **Merge Tool**.  
Pour ouvrir **Merge Tool**, faites un clic droit : **Team -> Merge Tool**.
Le conflit apparaît, on pourrait résoudre directement le conflit à partir du **Merge Tool**, mais on va préférer le résoudre manuellement.

Fermez donc le **Merge Tool** en cliquant sur la croix de l'onglet qui le contient et qui commence par `Repository testgit : ...`.

Vous devriez retrouver à l'écran le fichier `HelloWorld` avec ses marqueurs de conflits.
Résolvez ce conflit manuellement en modifiant directement le code du fichier `HelloWorld`.
Nous garderons dans ce fichier la ligne au dessus des ` =======` et les deux dernières lignes relatives à `MaraboutSimple`.  
Modifiez donc directement le code dans le fichier `HelloWorld` en supprimant les marqueurs et en ne gardant que les lignes de code pertinents de manière à obtenir

```Java

    public class HelloWorld {
	
	    public static void main(String[] args) 
	    {
		    System.out.println( "Hello World !" );
		    System.out.println( "Jouons ensemble!" );
		
		    System.out.println( "Et si on jouait au FizzBuzz ?" );  
            System.out.println(FizzBuzz.jouer());
        
            System.out.println( "Et si on jouait au Marabout ?" );  
            System.out.println(Marabout.jouer());
        
            System.out.println( "Et si on jouait au Marabout de manière plus simple ?" );  
            System.out.println(MaraboutSimple.getResult());
	}

}

```

Remarque : Il est bien évidemment qu'il faudra refactorer plus tard le getResult en jouer : ici nous nous contentons uniquement de résoudre le conflit avec les lignes de code qui nous étaient proposées ;-) ...

Sauvegardez et exécutez pour vérifier que votre programme fonctionne correctement !

Deux étapes sont ensuite nécessaire :

* **indiquer à git** que le conflit a été résolu en ajoutant le fichier en conflit (avec l'icone rouge) à l'index : depuis `HelloWorld.java`, clic droit : `Team->Add Index` . L'icône rouge disparait alors indiquant que le conflit est résolu. Le nom du projet s'est transformé en **`testgit [testgit|Merged master]`** . Il ne reste donc plus qu'à ...

* ... **effectuer un commit** pour que cette fusion soit bien pris en compte par un clic droit : **Team -> Commit**. Il est à noter que le message de commit est déjà pré-rempli. Il ne reste plus qu'à valider par **Commit** pour retrouver le nom de projet **`testgit [testgit master]`** signifiant que nous sommes bien revenu sur la branche **master** et donc que la fusion a été prise en compte, ce qui peut être vérifié en jetant un petit coup d'oeil dans la **vue `History** qui mentionne le dernier commit comme `Merge branch marabout-simple`. 

Remarque : Pour en savoir plus, sur la résolution de conflit lors d'une fusion avec le plug-in Egit, n'hésitez pas à consulter la rubrique **Resolving a merge conflict** dans le **wiki d'Eclipse** dédié à Egit disponible [ici](http://wiki.eclipse.org/EGit/User_Guide#Resolving_a_merge_conflict).

#### 7.10 Pousser sur le dépôt distant Github


Depuis la **vue `Git Repositories`** se positionner sur le **sous-noeud `https://github.com/username/mondepot.git` précédé de la flèche ROUGE**  de `origin` du `Remotes` du projet.

Clic droit : **`Push`**.   
  
Fenêtre de progression suivie de fenêtre `Push Results : testgit-origin` qui indique que l'opération a bien été effectuée (**`master -> master [new branch]`**).

Cliquer sur `OK`.

Rendez-vous via votre navigateur sur votre dépôt Github (https://github.com/username/testgit) pour vérifier que les fichiers soumis au gestionnaire de version ont bien été poussés correctement vers le dépôt distant.

Cliquez sur l'onglet **`Graph`** de votre projet, puis sur l'onglet **`Network`**, vous obtiendrez une représentation visuelle de votre historique git similaire à la suivante :

![Historique_MaraboutSimple_Fusion](images/Historique_MaraboutSimple_Fusion.png)
  
Remarque : Cette représentation permet de mieux visualiser  :

* les branches dont la fusion a été réalisée en **fast-forward** et qui ont donné lieu à un historique linéaire : c'est le cas des branches **fizzbuzz** ou **refactoring**  
* et les branches qui ont subi une *vraie fusion (**true merge** avec ou sans conflit) qui permet de garder un trace des branches dans l'historique (les branches apparaissant alors sous forme de *bosses*) : c'est le cas de **marabout** ou **marabout-simple**

<!-- Parle-t-on de la vue Git Staging -->

<!-- Peut-être serait-il interssant de jouer aussi avec la pull-request, il faudra alors créer une branche en local genre comptine : un deux tris nous irons au bois..puis pousser sous github sans fusionner...Une pull-request seriat alors proposé sur Github : à voir suivant le temps... -->


**Remarque** : *Pour information, pour **dissocier un projet existant de son dépôt git**, il suffirait de se placer sur le projet en question dans la vue `Package Explorer`, puis à partir d'un clic droit de sélectionner `Team -> Disconnect `. Le dépôt git existerait toujours, mais il ne serait plus attaché au projet Eclipse.*



## Comment contribuer à un projet existant sur Github ? (fork & co) <a id="fork"></a>

Voici quelques lectures pour vous aider et donner envie de à des projets existants sur Github :

* [Forking Projects](https://guides.github.com/activities/forking/)
* [Ma méthode de travail avec Git et GitHub](https://tech.mozfr.org/post/2016/04/16/Ma-methode-de-travail-avec-Git-et-GitHub) qui est une traduction de [Documenting my git/GitHub workflow](http://www.otsukare.info/2016/04/14/git-workflow)
* [Chapitre 6.2 : GitHub - Contribution à un projet](https://git-scm.com/book/fr/v2/GitHub-Contribution-%C3%A0-un-projet) du [Pro Git book](https://git-scm.com/book/fr/v2)



## Références <a id="references"></a>

Les références consultées pour l'écriture de ce tutoriel sont disponibles :  [ici](git_references.md)

 
... sans oublier ...
  
- Présentation du jeu FizzBuzz : [ici](https://en.wikipedia.org/wiki/Fizz_buzz)  
- Présentation du jeu Marabout  : [ici](https://fr.wikipedia.org/wiki/Marabout_(jeu))  
- Chanson *Trois petits chats* : [ici](https://fr.wikipedia.org/wiki/Trois_petits_chats)  




## On en discute ?
Pour les discussions, c'est par [là](https://github.com/iblasquez/tuto_git/issues)  
Pour les propositions de contenu, de modification par [ici](https://github.com/iblasquez/tuto_git/pulls)  
Et bien sûr, n'hésitez pas à personnaliser vos messages avec des [emojis](http://www.webpagefx.com/tools/emoji-cheat-sheet/) :smile:

## Licence

Ce document est placé sous licence CC BY-NC-SA :  [Creative Commons
Attribution - Pas d'Utilisation Commerciale - Partage dans les Mêmes Conditions](https://creativecommons.org/licenses/by-nc-sa/4.0/)

<img src="https://licensebuttons.net/l/by-nc-sa/3.0/88x31.png" width="100">

En savoir plus sur [les licences Creative Commons](https://creativecommons.org/licenses/?lang=fr-FR) ... 

Toutefois, toute personne enseignant au département Informatique de l'IUT du Limousin souhaitant utiliser ce document doit demander une autorisation préalable :smile:



