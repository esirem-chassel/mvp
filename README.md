# mvp

## Consigne générale

Nous allons créer un système de quizz __jouable dans la console__.

Pour le cadre de l'exercice, nous allons nous restreindre à un simple quizz codé "en dur", mais nous allons réaliser les structures de base.
Il est probable que vous n'arriviez pas à 100% des éléments demandés - ce qui sera étudié et noté sera votre progression, la cohérence de votre code, ainsi que la mise en oeuvre des différents compétences de programmation OO/C++. Des questions peuvent vous être posées pour vérifier cette mise en oeuvre.
Il est notemment attendu une bonne pratique de l'encapsulation, le respect des normes de base de programmation en C++ (nommage des propriétés, méthodes...) que nous avons vu en cours et en TD/TP.

> [!Caution]
> Vous **devez** créer un dépôt `3aa-mvp` sur Github et inviter votre enseignant sur le dépôt.
> 
> Il est impératif de réaliser au moins un commit par partie/question.
> 
> Chaque partie peut et doit être testée avant de passer à la partie suivante.
> 
> Les recherches sur documentation, Internet et consorts sont autorisées.
> 
> L'usage d'intelligence artificielle et la communication active avec des tiers est interdite.


## Parties de développement


### Interaction utilisateur - partie 1

Au début de votre programme principal, demandez à l'utilisateur s'il souhaite :
- `[J]ouer`
- `[Q]uitter`

Continuez à demander à l'utilisateur ce qu'il souhaite tant qu'il n'a pas saisi `J` ou `Q`.

Si l'utilisateur choisit `Q`, fermez l'application correctement.
Si l'utilisateur choisit `J`, affichez un simple message pour le moment.


### Interaction utilisateur - partie 2

Déplacez le code réalisé pour demander à l'utilisateur son choix dans une méthode `mainLoop` d'une classe `UI` que vous créerez.

Cette classe `UI` contiendra également une propriété `lang` par défaut à `fr_FR`.
Modifiez votre `main` en conséquence.

Dans la boucle où on demande à l'utilisateur s'il souhaite jouer ou quitter, ajoutez un nouveau choix `[L]angue`.

Si l'utilisateur choisit cette option, permettez-lui de saisir une langue au format international (ex : `fr_FR` pour le français en France, `en_US` pour l'anglais américain, etc.). On ne contrôlera pas la cohérence de la saisie.


### Création de la structure Question

Créez une classe Question, contenant une chaîne `_content` (le contenu de la question), une [liste (std::map) associative](#fonctionnement-de-stdmap) de clefs (caractère) et valeurs (chaînes), ainsi qu'un caractère indiquant quelle réponse est valide.

Ajoutez également une surcharge d'opérateur << pour permettre l'affichage de la question.

Dans votre classe `UI`, ajoutez, dans le constructeur, la construction d'[une question "test"](#question-test) et son affichage, afin de contrôler que la question fonctionne correctement.

> [!Note]
> Pensez à retirer votre test après votre commit et avant de passer à la partie suivante.


### Vérification de la réponse

Ajoutez, dans la classe Question, une méthode `verify` prenant en paramètre un caractère et renvoyant un booléen. Cette méthode renverra `true` si le caractère correspond bien à la bonne réponse, et `false` sinon.

Dans votre classe `UI`, après avoir demandé à l'utilisateur s'il souhaite jouer, affichez [une question "test"](#question-test), puis permettez de saisir une chaîne et vérifiez si cette chaîne correspond à la bonne réponse ou non.

> [!Note]
> Pensez à retirer votre test après votre commit et avant de passer à la partie suivante.


### Liste de questions

Dans votre classe `UI`, ajoutez une propriété contenant une liste (std::vector) de questions, ainsi qu'un entier `_score`, initialisé à zéro par défaut, pour garder en mémoire la progression dans la liste de questions.

Dans le constructeur de la classe `UI`, ajoutez [vos questions](#quizz-liste-des-questions).

Ajoutez, dans la classe `UI`, deux méthodes :
- une méthode `askQuestion`, prenant en paramètre une Question, et renvoyant un booléen (true si la question a bien été répondu, false sinon). Cette méthode va afficher la question, suivie des réponses possibles, puis va demander à l'utilisateur son choix. Le choix saisi correspondra à la clef de la réponse. On prendra soin de redemander à l'utilisateur si son choix n'existe pas dans les choix possibles.
- une méthode `play`, renvoyant un booléen, qui va boucler sur chaque question, et appeller `askQuestion`. A chaque question, si `askQuestion` renvoie true, on passera à la question suivante en incrémentant `_score`. Sinon, le jeu s'arrête, et le score final est affiché.

Dans votre code principal, lorsque l'utilisateur souhaite jouer, utilisez la méthode `play`.


### Création de la structure Réponse

Nous allons structurer nos réponses, en transformant chaque réponse en objet. Créez une classe Response, contenant une chaîne de caractères `_content`.
Modifiez vos différents codes pour inclure l'usage de Response là où cela est pertinent.


### Création de la structure Quizz

Nous allons structurer notre Quizz, afin de créer une classe Quizz, comprenant un nom `_content`, et une liste de questions.
Ensuite, modifiez votre classe `UI` pour conserver le Quizz plutôt qu'une liste de questions.

> [!Tip]
> Vous aurez besoin de créer des accesseurs et d'autres méthodes utilitaires dans votre structure Quizz.


### Création de la structure Translatable

Créez une classe Translatable, de laquelle vont hériter Quizz, Response et Question.
Cette classe parent va définir une liste (std::map) associant langue et contenu, en plus du contenu "par défaut". Si le contenu pour la langue choisie n'existe pas, on utilisera ce contenu par défaut.

Ajoutez dans cette classe une méthode statique `switchLang` qui prendra en paramètre la langue de l'utilisateur (qui peut être saisie par l'utilisateur dans la [partie utilisateurs 2](#interaction-utilisateur-partie-2).

Quelques conseils :
- ne définissez `_content` qu'une seule fois dans la classe `Translatable`
- prévoyez une méthode `getDisplay` dans la classe `Translatable`, qui permettra l'affichage en fonction de la langue choisie
- utilisez cette méthode `getDisplay` dans les surcharges de flux pour Quizz, Question et Response.
- rappellez-vous des visibilités/portées d'héritage !


### Création de la structure Storage

Créez une classe Storage contenant une propriété `fh`, qui sera un pointeur vers un [`fstream`](#lecture-écriture-de-fichiers-fstream).
Le fichier concerné se nomme `score.txt`.
L'objectif est de stocker dans ce fichier le score utilisateur obtenu lors d'une partie (on ne sauvegardera le score qu'à la fin de la partie, et on affiche le score au lancement du jeu).
Le score sera cumulatif au fil des parties.


## Annexes


### Question test

```md
Contenu : "Test"
Bonne réponse : C
Réponses :
A. Faux
B. Aussi Faux
C. Vrai
D. Toujours Faux
```


### Quizz (liste des questions)

```md
Contenu : Quel est le plus grand océan du monde ?
Bonne réponse : A
Réponses :
A. Océan Pacifique
B. Océan Atlantique
C. Océan Indien
D. Océan Arctique

Contenu : Qui a peint la fresque de la Cène ?
Bonne réponse : B
Réponses :
A. Michel-Ange
B. Léonard de Vinci
C. Raphaël
D. Botticelli

Contenu : Quelle est la capitale de l’Australie ?
Bonne réponse : C
Réponses :
A. Sydney
B. Melbourne
C. Canberra
D. Brisbane

Contenu : En quelle année a eu lieu la chute du mur de Berlin ?
Bonne réponse : D
Réponses :
A. 1985
B. 1991
C. 1980
D. 1989

Contenu : Quel est le symbole chimique de l’or ?
Bonne réponse : B
Réponses :
A. Ag
B. Au
C. Fe
D. Pb
```


### Documentation / Aide C++

Vectors : https://en.cppreference.com/w/cpp/container/vector

Maps : https://en.cppreference.com/w/cpp/container/map

Et bien d'autres !

### Rappel : lecture de flux

Vous **devez** importer <stdio> pour permettre l'usage des flux d'entrée et de sortie.
Pour lire une chaîne depuis la console (saisie utilisateur) :
```cpp
std::string myVar = "";
std::cin >> myVar;
```


### Fonctionnement de std::map 

La classe `std::map` (disponible en important `<map>`) est une classe permettant d'utiliser des tableaux associatifs.

```cpp
// créer une map (ici, les clefs sont des entiers et les valeurs sont des flottants
std::map<int, float> ls = {};
// ajouter un élément
ls[10] = 5f;
// vérifier qu'un élément existe
if(ls.count(10)) {
    // lire un élément
    ls.at(10);
}
```

### Lecture/écriture de fichiers : fstream

Un `fstream` est un objet permettant la lecture et/ou l'écriture de fichier.
Lors de l'ouverture du fichier, à l'initialisation de l'objet fstream, vous devez fournir :
- le nom du fichier concerné
- le mode d'ouverture, qui est un flag composé
- - le flag std::ios::in indique une ouverture en écriture
  - le flag std::ios::out indique une ouverture en lecture
  - le flag std::ios::app indique de positionner le curseur au début du fichier
- les flags sont composés via l'opérateur binaire OU `|` 

La méthode `seekp` permet de positionner le curseur de lecture / écriture.

La méthode `getline` de `std`, prenant en premier paramètre le `fstream` et en second paramètre la chaîne dans laquelle écrire, permet de lire une ligne de fichier et placer le résultat dans la chaîne.

Enfin, pour écrire dans le fichier, vous pouvez utiliser l'opérateur de flux `<<` pour "injecter" une chaîne dans le fichier.


### Représentation UML

![Représentation UML](/uml_mvp.png)

> [!Important]
> Ne sont pas représentés les types de retour de certaines méthodes : à vous d'y réflechir.
> De même, la majorité des accesseurs et constructeurs sont absents, c'est à vous de les ajouter au besoin.
