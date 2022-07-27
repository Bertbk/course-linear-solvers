**************
Stockage Dense
**************

Prise en main des :code:`class` en C++ pour créer :

1. une classe :code:`Vecteur` pour les vecteurs
2. une classe :code:`Matrice` pour les matrices carrées

Dans cette partie, le stockage des matrices sera *dense*, c'est-à-dire que chaque coefficient sera stocké en mémoire, qu'il soit nul ou pas. Le coût mémoire d'une matrice carré de taille :math:`N` est donc de :math:`O(N^2)`.

.. proof:tips::

  - Créez un fichier :code:`dense.cpp` à la racine pour effectuer des tests
  - Modifiez le :code:`Makefile` pour compiler le fichier :code:`dense.cpp`
  - Chaque nouvelle fonction (ou fonctionnalité) devra être **testée** et **validée**, via par exemple le fichier :code:`dense.cpp` (libre à vous de créer plus de fichiers exécutables !)


.. toctree::
  :maxdepth: 1

  vector
  matrix
  io
  test-matrices
  cost