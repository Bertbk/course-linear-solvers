.. _sec-diag-system:

Systèmes Diagonaux
==================

Objectif
--------

Résoudre un système linéaire diagonal.

Préparation
-----------

Comme les fonctions présentés dans ce chapitre font intervenir à la fois les matrices et les vecteurs, il semble judicieux de les placer dans des fichiers dédiés. Vous pouvez créer un fichier source et un fichier en-tête réservées aux solveurs directs, par exemple :code:`include/direct_solvers.hpp` et :code:`src/direct_solvers.cpp`. Cela permet de mieux compartimenter les fonctions. N'oubliez pas, alors, d'inclure les fichiers en-tête dont vous avez besoin (:code:`Matrice.hpp` ? :code:`Vecteur.hpp` ? ... ?)

Problème
--------

Nous considérons un système linéaire :math:`AX = b` où la matrice :math:`A` est diagonale. L'inverse d'une telle matrice est également diagonal et est simple à calculer. Il nous suffirait ensuite de multiplier :math:`A^{-1}` par :math:`b` pour obtenir :math:`X`. Par exemple :

.. math:: 

  A = \begin{pmatrix}
  a & 0 & 0 & 0 \\
  0 & b & 0 & 0 \\
  0 & 0 & c & 0 \\
  0 & 0 & 0 & d \\
  \end{pmatrix}
  ,\quad
  A^{-1} = \begin{pmatrix}
  \frac{1}{a} & 0 & 0 & 0 \\
  0 & \frac{1}{b} & 0 & 0 \\
  0 & 0 & \frac{1}{c} & 0 \\
  0 & 0 & 0 & \frac{1}{d} \\
  \end{pmatrix}

Cette méthode présente cependant plusieurs problèmes importants :

1. Stocker :math:`A^{-1}` signifie doubler la :ref:`quantité de mémoire nécessaire <sec-cpu-cost>`, ce que nous refusons
2. Un produit matrice(dense)-vecteur coûte :math:`O(N^2)` opérations, or la matrice dispose d'une structure particulière que nous pourrions utiliser.


Algorithme de résolution
------------------------

Un calcul simple montre que la solution :math:`X` est donné par :

.. math::

  X(i) = \frac{b(i)}{A(i,i)}, \quad\forall i=0,\ldots, N-1.

Plutôt que d'inverser la matrice :math:`A`, dont on a vu que ce n'était pas une bonne idée, nous proposons d'implémenter une fonction qui prend en argument :math:`A` et :math:`b` et qui fournit en retour le résultat :math:`X = A^{-1}b`. Son pseudo code est alors le suivant :

.. code-block:: cpp

  Vecteur X(n);
  for (int i =0; i < n; i++)
    X(i) = b(i)/A(i,i);

.. proof:exercise::

  Quelle est la complexité d'un tel algorithme ?

.. proof:remark::

  Nous cherchons :math:`X (= A^{-1}b)` et non :math:`A^{-1}` ! C'est une nuance (très) importante et qu'il faut garder en tête tout au long des TPs !

Implémentation
--------------

Implémentez une fonction qui prend en argument une :code:`Matrice A`, **supposée diagonale**, et un :code:`Vecteur b`. Cette fonction renvoie le vecteur solution :math:`X` tel que :math:`AX = b` :

.. code-block:: cpp

  Vecteur solve_diag(const Matrice &A, const Vecteur& b);


.. proof:warning::

  **Ne vérifiez surtout pas** si la matrice est bien diagonale, supposez le uniquement :

  - La vérification coûte cher
  - Cela serait même nuisible pour la suite
