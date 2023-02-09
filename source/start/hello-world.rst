.. _start-hello-world:

Hello World!
============

Objectifs
---------

1. Se remémorer/apprendre rapidement l'usage du terminal
2. Compiler un programme simple en C++ en utilisant le terminal


Terminal
--------

Par *Terminal*, nous faisons référence à une interface graphique qui permet de lancer des commandes tapées au clavier. Ces commandes permettent d'interagir avec le système en lui donnant des instructions dont le résultat peut être éventuellement affiché dans le terminal.

Nous utiliserons des commandes usuelles pour les terminaux des systèmes Unix (Linux et macOS par exemple). Libre à vous d'utiliser un autre système d'exploitation cependant, comme précisé dans l'introduction, nous n'assurerons alors pas le SAV. D'après notre expérience, ce sont les systèmes d'exploitation les plus utilisés dans le monde de la recherche et de la R&D en calcul scientifique. Par exemple, `498 des 500 supercalculateurs les plus puissants dans le monde utilisent un système Unix en 2014 <http://www.zdnet.fr/actualites/linux-accro-t-encore-sa-domination-des-supercalculateurs-39802945.htm>`_.

Voici une liste non exhaustive de commandes à connaître pour ce cours (c'est un minimum, n'hésitez pas à être curieuse/curieux) :

- :code:`pwd` : donne la position du répertoire courant dans l'arborescence
- :code:`ls` : affiche le contenu du répertoire courant
- :code:`cd` :code:`dossier` : positionne le répertoire courant à :code:`dossier`
- :code:`mkdir` :code:`dossier` : créer un dossier :code:`dossier` dans le répertoire courant
- :code:`touch` :code:`fichier` : créer un fichier vide nommé :code:`fichier`
- :code:`more` :code:`fichier` : affiche le contenu du :code:`fichier`

.. proof:remark::

  Un autre raccourcis clavier très utile est :code:`Ctrl + C`  pour annuler le programme en cours de lancement dans le terminal (pour mettre fin à une boucle infinie par exemple).


.. proof:exercise::

  À vous de jouer :

  1. Ouvrez un terminal
  2. Utilisez la commande :code:`pwd` pour vous situer dans l'arborescence
  3. Déplacez vous à l'endroit de votre choix à l'aide de :code:`cd`
  4. Créez un répertoire :code:`Test` avec la commande :code:`mkdir Test`

.. proof:remark::

  Notez que :

  - La plupart des commandes Unix possèdent des options. Par exemple, en utilisant :code:`ls -a` alors tout le contenu du répertoire courant, même les fichiers cachés, est affiché.
  - Pour avoir plus d'informations sur une :code:`commande` et ses options, on peut utiliser la commande suivante : :code:`man commande`
  - `Aide mémoire <http://www.generation-linux.fr/dl/Les_commandes_linux.pdf>`_

.. _subsec-compilation:

Compilation
-----------

Supposons que nous avons un fichier :code:`hello_world.cpp` dans le répertoire :code:`Test` qui correspond au code source d'un programme qui affiche :code:`Hello world` lors de son exécution. La manière la plus simple pour compiler un tel programme est d'ouvrir le terminal, de se positionner dans le répertoire :code:`Test` et d'utiliser la commande suivante :

.. code-block:: bash

  g++ hello_world.cpp

Cette commande appele le **compilateur** C++ (ici, :code:`g++`) afin de créer le programme correspondant. Plus précisément, le compilateur crée à partir du code source un **fichier objet** :code:`hello_world.o`, ce dernier contient des instructions que le processeur comprend. En d'autres termes, il traduit le code source en langage machine. Puis, il génère l'exécutable nommé :code:`a.out`. Remarquez que dans le cas où il y aurait plusieurs fichiers sources, le compilateur les lie afin de créer l'exécutable durant l'étape `d'édition des liens <https://fr.wikipedia.org/wiki/%C3%89dition_de_liens>`_. Il ne reste plus qu'à exécuter le programme de la façon suivante :

.. code-block:: bash

  ./a.out


.. proof:remark::

  Remarquez que :

  - Le nom de l'exécutable (":code:`a.out`") peut être modifié de la manière suivante à l'aide de l'option :code:`-o` suivi du nom de l'exécutable :

  .. code-block:: bash
  
    g++ nom_fichier.cpp -o nom_executable
  
  -  Les deux étapes de compilation peuvent être effectuées séparément. La commande suivante met en oeuvre la compilation et la génération du fichier objet en précisant avec l'option :code:`-c` de ne pas effectuer l'édition des liens et avec l'option :code:`-o` le nom du fichier objet :

  .. code-block:: bash

    g++ -c nom_fichier.cpp -o nom_fichier.o

  Puis l'édition des liens et la génération de l'exécutable sont effectués ainsi :

  .. code-block:: bash

    g++ nom_fichier.o -o nom_executable


.. proof:tips:: 

  Il existe de nombreuses options pour la commande :code:`g++`. En voici quelques unes :

  - :code:`-Wall` permet d'afficher un maximum d'alertes ("Warning all").
  - :code:`-g` active les options de débogage et permet notamment l'utilisation d'un débogueur. Attention, cela ralentit considérablement la compilation et l'exécution : à n'utiliser que pour déboguer.
  - :code:`-O2` ou :code:`-O3` permet certaines optimisations lors de la compilation.
  - Le compilateur peut émettre une liste d'erreurs qu'il convient alors de corriger pour qu'il puisse créer l'exécutable. Il va vous falloir apprendre à lire ces erreurs.
  - :code:`std=c++11` utilise les fonctions du standard :code:`C++11`


Hello world
-----------

Créons un fichier C++ qui contiendra le code source de notre premier programme. Pour cela, nous avons besoin d'un (bon) :ref:`éditeur de texte <sec-software>`. Rappelons que le cœur d'un programme C++ est la fonction :code:`main` qui sera appelée au lancement du programme qui sera issue du code source.

Saisie et affichage dans le terminal : :code:`cin` et :code:`cout`
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Les fonctions de saisie et d'affichage, respectivement :code:`cin` et :code:`cout`, font partie de la bibliothèque standard, elles font donc partie de l'espace des noms (*namespace*) :code:`std`. Autrement dit, pour les appeler nous devons écrire

.. code-block:: cpp

  std::cin  >> ma_variable; // Saisie (clavier)
  std::cout << "Coucou !!"; // Sortie

Ces fonctions sont déclarées dans la bibliothèque :code:`iostream` que l'on doit inclure au début du fichier. Notre code ressemble donc à ceci :

.. code-block:: cpp
  
  #include <iostream> // Entrees/Sorties

  using namespace std; // Facultatif

  int main(){
    ...
    // Votre code
    ...
  }

Notez qu'à la ligne 3 du code précédent, nous avons rajouté :code:`using namespace std`, qui permet de rendre le *namespace* :code:`std` implicite dans tout le document et l'on peut appeler directement :code:`cout` ou :code:`cin` sans le préfixe :code:`std::`.

.. proof:warning::
  
  Ne placez **jamais** :code:`using namespace` dans un fichier header (un fichier :code:`.h` ou :code:`.hpp`) ! `Pour en savoir plus <https://stackoverflow.com/questions/5849457/using-namespace-in-c-headers>`_. 

  De plus et bien que courant, utiliser :code:`using namespace std` est `une mauvaise habitude <https://stackoverflow.com/a/1453605/14065)>`_.

.. proof:tips::

  Vous trouverez la syntaxe de :code:`cin` et :code:`cout` en ligne sur, par exemple :

  - `Openclassroom (fr) <https://openclassrooms.com/fr/courses/1421911-du-c-au-c/1422044-premier-programme-c-avec-cout-et-cin>`_
  - `Cplusplus.com (eng) <http://www.cplusplus.com/doc/tutorial/basic_io/>`_

.. proof:exercise::

  Implémentez ce qui suit ("Hello World! ").

  1. Créez un fichier :code:`hello_world.cpp` (avec votre éditeur de texte ou la commande :code:`touch`)
  2. Dans ce fichier, écrivez un programme qui affiche :code:`Hello world!` dans le terminal
  3. Compilez votre code source (:ref:`voir section sur la compilation <subsec-compilation>`) et vérifiez que l'exécutable a bien le comportement attendu

Une fois l'exercice effectué, libre à vous de faire les exercices suivants pendant la séance de TP. Mais si vous êtes à l'aise, vous pouvez :ref:`passer à la suite <sec-compilation>`.

.. proof:exercise::

  Facultatif :

  - De la même façon que dans l'exercice précédent, créez un exécutable qui permette à l'utilisateur de saisir deux entiers et qui renvoie la somme de ces derniers.
  - Modifier le code source pour que l'exécutable effectue la somme de deux entiers positifs et qu'il renvoie une erreur de saisie à l'utilisateur lors de la saisie d'un entier négatif.
  - Créez un exécutable tel que l'utilisateur puisse saisir autant de nombres qu'il le souhaite et que l'exécutable en affiche la somme. (indice : vous pouvez demander en premier à l'utilisateur combien d'entiers il veut saisir par exemple).
  - À l'aide des deux dernières questions, faites en sorte que l'utilisateur puisse saisir autant d'entiers positifs qu'il le souhaite et que l'exécutable renvoie une erreur de saisie si un entier négatif est donné.

.. proof:exercise::

  `À l'aide de la commande switch <https://en.cppreference.com/w/cpp/language/switch>`_, créez un exécutable qui permette à l'utilisateur de saisir un entier positif et qui renvoie le jour de la semaine correspondant si cet entier est entre 1 et 7. Dans le cas contraire, l'exécutable devra afficher un message précisant que l'entier saisi ne correspond à aucun jour.
