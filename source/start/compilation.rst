.. _sec-compilation:

Compilation : plus de détails
=============================

Objectifs
---------

1. Apprendre à séparer les **définitions** des **déclarations** des fonctions
2. Prémisse de structure du projet (*workflow*)

Séparer la déclaration et la définition
---------------------------------------

En C++, il est commun de séparer "physiquement" la **déclaration** des fonctions de leur **définition**. La déclaration fournit au compilateur les informations sur les arguments entrants et sortants de la fonction (*quel type ?*), tandis que la définition détaille le fonctionnement interne de la fonction (*comment ça marche ?*). En pratique, les déclarations sont stockées dans un fichier :code:`.h` (ou :code:`.hpp`), appelé *fichier d'en-têtes* ou *fichier header* (ou *header*), tandis que les définitions le sont dans les fichiers :code:`.cpp`, appelés *fichiers sources*. 

Reprenons l'exemple du :code::code:`Hello World` et réécrivons le dans la structure proposée. Au lieu de disposer d'un seul fichier, nous en avons maintenant trois :

1. :code:`hello.cpp` contiendra les définitions des fonctions dont nous avons besoin
2. :code:`hello.hpp` contiendra les déclarations des fonctions dont nous avons besoin (les *header*)
3. :code:`main.cpp` qui contiendra le programme à exécuter

Et nous avons alors, dans l'ordre

.. code-block:: cpp

  // fichier hello.hpp
  #pragma once // voire remarque ci-dessous
  void hello_world(); // Déclaration de la fonction

.. code-block:: cpp

  // fichier hello.cpp
  #include "hello.hpp"
  #include <iostream> //cin et cout
  // Déclaration de la fonction
  void hello_world(){
    std::cout << "Hello World!";
  }

.. code-block:: cpp
  
  // fichier main.cpp
  #include "hello.hpp"

  int main(){
    hello_world(); //appel de la fonction
  }

.. proof:remark::

  Afin d'éviter que le fichier d'en-tête soit inclus plusieurs fois à travers le projet, les fichiers d'en-tête :code:`.hpp` commenceront systématiquement par :code:`# pragma once`. Remarquez que cette commande `nécessite d'utiliser le standard C++ 11 <https://stackoverflow.com/questions/10363646/compiling-c11-with-g>`_ (ce que nous encourageons !):

  .. code-block:: bash

    g++ -std=c++11


.. proof:tips::

  Nous pouvons utiliser des chevrons ou des guillemets pour les `#include` :

  .. code-block:: cpp

    #include <tableau.hpp> // chevrons
    #include "tableau.hpp" // guillemets

  La différence réside dans `la recherche du fichier effectuée par le compilateur <https://stackoverflow.com/questions/21593/what-is-the-difference-between-include-filename-and-include-filename>`_ :

  - Chevrons : la recherche s'effectue dans les dossiers désignés par le pré-processeur. C'est donc cela que nous utilisons pour les bibliothèques standards.
  - Guillemets : le compilateur recherche d'abord dans le répertoire courant (celui du fichier appelant) puis, en cas d'échec, suit la règle de recherche des chevrons. Les guillemets sont (en général) à privilégier pour vos fichiers d'en-tête.

Compilation
-----------

Compilons maintenant notre petit projet avec la commande suivante :

.. code-block:: bash

  g++ main.cpp hello.cpp -o main -std=c++11

Ou alors nous pouvons générer les fichiers objets des différents fichiers source puis créer l'exécutable à partir des sources :

.. code-block:: bash

  # Compilation de hello
  g++ -c hello.cpp -o hello.o -std=c++11
  # Compilation du main
  g++ -c main.cpp -o main.o -std=c++11
  # Édition des liens
  g++ main.o hello.o -o main -std=c++11
  # Lancement de l'exécutable
  ./main

.. proof:exercise::

  En vous inspirant fortement de ce qui précère, vous devez construire une fonction qui rempli `un tableau de type std::vector<int> <https://fr.cppreference.com/w/cpp/container/vector>`_ avec des nombres de 1 à n :

  - Créez deux fichiers :code:`tableau.cpp` et :code:`tableau.hpp`
  - Dans le fichier d'en-tête (header), déclarez une fonction de prototype : 
    
  .. code-block:: cpp

    void remplissage(std::vector<int> &tableau, int n);

  Vous aurez besoin d'ajouter en haut du fichier :

  .. code-block:: cpp

    #include <vector>
  
  - Définissez dans le fichier source associé. La fonction doit redéfinir la taille du tableau :code:`tableau` à :code:`n` et le remplir de nombres entiers [#f1]_ de 1 à n.
  - Appelez cette fonction dans le fichier :code:`main.cpp`.
  - Compilez et exécutez le programme.
  - Vérifiez que ça fonctionne en affichant chaque valeur du tableau.

.. rubric:: Footnotes

.. [#f1] utilisez de préférence les méthodes `clear() <www.cplusplus.com/reference/vector/vector/clear/>`_ et `push_back() <http://www.cplusplus.com/reference/vector/vector/push_back/>`_