
Factorisation LU : Pivotage
===========================

Objectifs
---------

Modifier la factorisation LU pour prendre en compte le pivot

.. proof:remark::

  Bien qu'extrêmement intéressant, cette partie est "optionnelle" : privilégiez d'abord les méthodes itératives avant de vous plonger dans la LU avec pivot !

.. proof:warning::

  Votre programme de factorisation LU doit être **fonctionnel** et **effectué dans la matrice** avant d'aller plus loin ! Autrement dit, vous l'avez **validé** sur une ou des systèmes linéaires !

Principe
--------

Lors du calcul de la factorisation LU, le pivot, c'est-à-dire le premier coefficient de la sous-matrice (complément de Schur) :math:`S_{k,k}(0,0)` peut être nul. Pour obtenir une factorisation LU, une méthode consiste à chercher un coefficient non nul dans cette sous-matrice et à *pivoter* les lignes et colonnes pour lui donner le rôle de pivot. 

.. proof:tips::

  L'unicité de la factorisation LU est perdue, celle-ci dépendant du choix du pivot.


Pivotage partiel
----------------

Nous faisons ici le choix de chercher le remplaçant du pivot parmi tous les coefficients de la sous-colonne uniquement. Ce nouveau pivot est le coefficient le plus grand (en terme de valeur absolue) et, en cas d'égalité, celui de plus petit indice ligne (*i.e* le "premier rencontré"). Le pivot n'est recherché que dans la première colonne c'est pourquoi nous parlons de **pivotage partiel**. Quand le pivot est cherché dans toutes la sous-matrices, nous parlons alors de **pivotage complet**.


Exemple
-------

Prenons le système linéaire suivant :

.. math::

  \underbrace{\begin{pmatrix}
    2&4&1&-3\\
  -1& -2&  2 & 4\\
    4&  2& -3&  5\\
    5& -4& -3& 1
  \end{pmatrix}}_{A}
  X = 
  \underbrace{\begin{pmatrix}
  1\\ 2\\ 3\\ 4
  \end{pmatrix}}_{b}

Effectuons maintenant la factorisation LU de :math:`A`  *sur place* :

.. raw:: html

  <div class="main-carousel" data-flickity='{ "draggable": false, "isWrapped": false, "selectedAttraction": "1", "friction": "1"}'>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_0.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 0
  :align: center

  Algorithme LU avec pivot : étape 0

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_1.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 1
  :align: center

  Algorithme LU avec pivot : étape 1
.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_2.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 2
  :align: center

  Algorithme LU avec pivot : étape 2

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_3.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 3
  :align: center

  Algorithme LU avec pivot : étape 3

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_4.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 4
  :align: center

  Algorithme LU avec pivot : étape 4

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_5.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 5
  :align: center

  Algorithme LU avec pivot : étape 5

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_6.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 6
  :align: center

  Algorithme LU avec pivot : étape 6

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_7.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 7
  :align: center

  Algorithme LU avec pivot : étape 7

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_8.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 8
  :align: center

  Algorithme LU avec pivot : étape 8

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_9.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 9
  :align: center

  Algorithme LU avec pivot : étape 9

.. raw:: html

  </div>
  <div class="main-carousel-cell" style="width: 100%;">

.. figure:: /img/lu_pivot/lu_pivot_10.*
  :figwidth: 100%
  :width: 100%
  :alt: Algorithme LU avec pivot : étape 10
  :align: center

  Algorithme LU avec pivot : étape 10

.. raw:: html
  
  </div>
  </div>

.. proof:remark::

  Il est possible aussi de ne pas pivoter directement les lignes de la matrice mais de conserver dans une matrice :math:`P` les différentes permutations utilisées. Cela permet d'éviter des opérations coûteuses sur la matrice, cependant la méthode proposée a le mérite de fonctionner et d'être relativement simple. 

Implémentation
--------------

Permutation de lignes : `Vecteur` et `Matrice`
++++++++++++++++++++++++++++++++++++++++++++++

Dans les classes :code:`Vecteur` et :code:`Matrice`, ajoutez deux méthodes permettant de pivoter intégralement deux lignes de prototype suivant :

.. code-block:: cpp

  void Vecteur::permute(int i, int j);
  void Matrice::permute(int i, int j);

LU avec pivot
+++++++++++++

Implémentez maintenant la factorisation LU avec pivot dont le prototype peut être sous forme :

- D'une méthode de la classe `Matrice` :

  .. code-block:: cpp

    void Matrice::lu_pivot(Vecteur &b);

- D'une fonction hors classe :

  .. code-block:: cpp

    void lu_pivot(Matrice &A, Vecteur &b);

L'algorithme ressemble alors au suivant :

.. code-block::

  Avant chaque factorisation partielle :
    - Vérifier si le pivot est nul (ou proche de zéro !)
      - Si oui :
        - Chercher le pivot (max de la sous-colonne) : sa valeur et son indice ligne
        - Pivoter les deux lignes dans la `Matrice` et dans le `Vecteur` membre de droite
      - Continuer la factorisation (comme précédemment)

.. proof:remark::

  La comparaison :code:`double a == 0` est très risquée en :code:`C++` du fait des approximations numériques : une quantité théoriquement nulle peut ne pas l'être tout en étant extrêmement petit, de l'ordre de 1e-16. Ainsi, quand vous vérifiez que le pivot est nul, préférez garder une tolérance, par exemple :

  .. code-block:: cpp

    if( abs(pivot) < 1e-14)   //Pivot considéré comme nulle

  Notons qu'un pivot très petit peut entrainer des instabilités numériques (car on divise par celui-ci). Il peut dès lors être utile de choisir une tolérance plus grande que 1e-14 ou de rechercher systématiquement le plus grand pivot, mais cela augmente le nombre d'opérations effectuées.
