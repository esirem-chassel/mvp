# mvp

## Consigne générale

Nous allons créer un système de quizz.
Pour le cadre de l'exercice, nous allons nous restreindre à un simple quizz codé "en dur", mais nous allons réaliser les structures de base.
Il est probable que vous n'arriviez pas à 100% des éléments demandés - ce qui sera étudié et noté sera votre progression, la cohérence de votre code, ainsi que la mise en oeuvre des différents compétences de programmation OO/C++. Des questions peuvent vous être posées pour vérifier cette mise en oeuvre.
Vous **devez** créer un dépôt `3aa-mvp` et inviter votre enseignant sur le dépôt.
Il est impératif de réaliser au moins un commit par partie/question.


## Interaction utilisateur - partie 1

Au début de votre programme principal, demandez à l'utilisateur s'il souhaite :
- `[J]ouer`
- `[Q]uitter`
Continuez à demander à l'utilisateur ce qu'il souhaite tant qu'il n'a pas saisi `J` ou `Q`.
Si l'utilisateur choisit `Q`, fermez l'application correctement.
Si l'utilisateur choisit `J`, affichez un simple message pour le moment.


## Interaction utilisateur - partie 2

Déplacez le code réalisé pour demander à l'utilisateur son choix dans une méthode `mainLoop` d'une classe `UI` que vous créerez.
Cette classe `UI` contiendra également une propriété `lang` par défaut à `fr_FR`.
Modifiez votre `main` en conséquence.
Dans la boucle où on demande à l'utilisateur s'il souhaite jouer ou quitter, ajoutez un nouveau choix `[L]angue`.
Si l'utilisateur choisit cette option, permettez-lui de saisir une langue au format international (ex : `fr_FR` pour le français en France, `en_US` pour l'anglais américain, etc.). On ne contrôlera pas la cohérence de la saisie.


## Création de la structure Question

Créez une classe Question, contenant une chaîne (le contenu de la question), une liste (std::map) associative de clefs (caractère) et valeurs (chaînes), ainsi qu'un caractère indiquant quelle réponse est valide.
Ajoutez également une surcharge d'opérateur << pour permettre l'affichage de la question.
Dans votre classe `UI`, ajoutez, dans le constructeur, la construction d'une question et son affichage, afin de contrôler que la question fonctionne correctement.


## Liste de questions

Dans votre classe `UI`, ajoutez une propriété contenant une liste (std::vector) de questions, ainsi qu'un entier, initialisé à zéro par défaut, pour garder en mémoire la progression dans la liste de questions.
Dans le constructeur de la classe `UI`, ajoutez vos questions.


## Création de la structure Réponse


## Création de la structure Quizz


## Création de la structure Translatable

Créez une classe Translatable, de laquelle vont hériter Response et Question.
Cette classe parent va définir une liste (std::map) associant langue et contenu, en plus du contenu "par défaut".
Quelques aides :
- faites

## Création de la structure Bonus

## Création de la structure Storage

Créez une classe Storage contenant une propriété `fh`, qui sera un pointeur vers un `fstream`.
Le fichier concerné se nomme `score.txt`.

> [!Note]
> Un `fstream` est un objet permettant la lecture et/ou l'écriture de fichier.
> Lors de l'ouverture du fichier, à l'initialisation de l'objet fstream, vous devez fournir :
> - le nom du fichier concerné
> - le mode d'ouverture, qui est un flag composé
> - - le flag std::ios::in indique une ouverture en écriture
>   - le flag std::ios::out indique une ouverture en lecture
>   - le flag std::ios::app indique de positionner le curseur au début du fichier
> - les flags sont composés via l'opérateur binaire OU `|` 
> 
> La méthode `seekp` permet de positionner le curseur de lecture / écriture.
> La méthode `getline` de `std`, prenant en premier paramètre le `fstream` et en second paramètre la chaîne dans laquelle écrire, permet de lire une ligne de fichier et placer le résultat dans la chaîne.
> Enfin, pour écrire dans le fichier, vous pouvez utiliser l'opérateur de flux `<<` pour "injecter" une chaîne dans le fichier.


## Annexes

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

