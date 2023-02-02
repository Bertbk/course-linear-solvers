.. _dense-io:

Input / Output
==============

Objectifs
---------

1. Construire des :code:`Vecteur` à partir d'un fichier
2. De même pour les :code:`Matrice`

Problématique
-------------

Construire des :code:`Vecteur` et des :code:`Matrice` est maintenant possible via les opérateurs parenthèses :

.. code-block:: cpp

  Matrice A(5);
  A(0,0) = 2;
  A(0,1) = -1;
  ...

Cette méthode peut s'avérer longue, notamment pour construire des matrices sans réelles *pattern* (ou schéma) pour lesquelles l'automatisation peut s'avérer compliquée. D'autre part, la matrice peut avoir été construire *via* un autre logiciel ou une autre source comme `Matrix Market <https://math.nist.gov/MatrixMarket/>`_.

:code:`Vecteur`
---------------

Nous proposons le format de fichier à deux colonnes : 

- La première ligne contient le nombre de lignes du :code:`Vecteur`
- Chaque ligne suivante contient 2 valeurs : l'indice (:code:`int`) et la valeur du coefficient (:code:`double`)
 
Par exemple pour le vecteur :math:`[1, 1.1, 1.2, 1.3, 0, 1.4]^T`

.. code-block:: bash

  6
  0 1.
  1 1.1
  2 1.2
  3 1.3
  4 0.
  5 1.4

:code:`Matrice`
---------------

Nous proposons le format similaire à `celui de Matrix Market <https://math.nist.gov/MatrixMarket/formats.html#mm>`_. Similaire aux :code:`Vecteur`, nous avons :

- La première ligne contient le nombre de lignes de la :code:`Matrice`
- Chaque ligne est ensuite composée de trois nombres : indice ligne, indice colonne et la valeur du coefficient. 

Par exemple pour la matrice suivante :

.. math::

  A = \begin{pmatrix}
  1.1 & 2. & 0\\\\\\
  0 & 5 & 0 \\\\\\
  0 & 2.3 & 3 
  \end{pmatrix}

serait sauvegardée dans le fichier suivant :

.. code-block:: bash

  3
  0 0 1.1
  0 1 2.
  0 2 0.
  1 0 0.
  1 1 5.
  1 2 0.
  2 1 0.
  2 1 2.3
  2 2 3.

.. proof:tips::

  Nous pouvons aussi sauvegarder de l'espace disque en ne sauvegardant pas les coefficients nuls ! Du fait des erreurs d'arrondi numérique, un coefficient peut être nul théoriquement mais pas dans la pratique. La condition `if(a == 0.)` est souvent à éviter, source d'erreurs. Nous préférerons utiliser une tolérance :

  .. code-block:: cpp

    #include <cmath>
    [...]
    double a;
    [...]
    if (abs(a) < 1e14)
      // est considéré comme nul
    else
      // non nul

Implémentation
--------------

Pour les classes :code:`Matrice` et :code:`Vecteur`, nous vous conseillons vivement d'implémenter des méthodes permettant de :

1. Lire des fichiers aux formats présentés plus haut et modifier l'objet appelant en fonction
2. Sauvegarder une :code:`Matrice` ou un :code:`Vecteur` sur disque au format proposé (pour une réutilisation future)

Vous pouvez aller plus loin en implémentant un constructeur qui prend en argument un nom de fichier et construit la :code:`Matrice` ou le :code:`Vecteur` associé. Il s'utiliserait alors de la façon suivante (exemple !) :

.. code-block:: cpp

  Matrice A("laplacien_A.txt");
  Vecteur b("laplacien_b.txt");
