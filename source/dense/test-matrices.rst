.. _sec-test-matrices:

Matrice Test
============

Objectif
--------

Implémenter des fonctions permettant de construire rapidement des matrices de test.

Matrice du Laplacien
--------------------

Afin de valider nos solveurs linéaires, nous avons besoin d'une matrice teste. Nous faisons le choix de la matrice tribande symétrique et définie positive $A_N$ de taille $N\times N$ :

.. math::
  :label: eq-LapN

  A_N =
  \begin{pmatrix}
    2 & -1 & 0 & 0 & \ldots & 0 & 0\\
    -1 & 2 & -1 &  0 & \ldots & 0 & 0\\
      0 & -1 & 2 & -1 & \ldots & 0 & 0 \\
      \vdots & \ddots& \ddots& \ddots & \ldots & \vdots  & \vdots\\
      0 & 0 & 0 & 0 & \ldots & -1 & 2 \\
  \end{pmatrix}

.. proof:exercise::

  Implémenter une fonction renvoyant une `Matrice`, prenant en argument :math:`N` et retournant la matrice :math:`A_N` donnée par l'équation :eq:`eq-LapN`. Plutôt qu'une fonction, vous pouvez implémenter une méthode de la classe :code:`Matrice` modifiant la Matrice appelante par :math:`A_N`.
