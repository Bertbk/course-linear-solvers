Factorisation LU
================

Objectifs
---------

1. Calculer la factorisation LU d'une matrice
2. Résoudre le système linéaire une fois la factorisation effectuée

Principe
--------

Cette méthode permet de transformer une matrice carré :math:`A` en un produit d'une matrice triangulaire inférieur :math:`L` et d'une matrice triangulaire supérieur :math:`U`. Cette décomposition permet notamment de résoudre des problèmes d'algèbre linéaire du type

.. math::
  :label:eq-Axb

  AX=b \iff LUX = B

où :math:`X` et :math:`B` sont respectivement les vecteurs solution et second membre. Au final, la solution :math:`X` est obtenu par deux résolutions successives :

.. math::

  X = U^{-1}(L^{-1}B).


Ainsi, comme :math:`L` et :math:`U` sont triangulaires respectivement inférieur et supérieur, les trois étapes pour résoudre le système :eq:`eq-Axb` sont :

1. Calculer la **factorisation LU**
2. Résoudre un système linéaire **triangulaire inférieur** (avec des 1 sur la diagonale)
3. Réoudre un système linéaire **triangulaire supérieur**

C'est parfait, nous avons déjà implémenté :ref:`les fonctions de résolution <sec-triangular-system>`, il ne nous manque plus que le calcul de la factorisation LU !

Factorisations partielle et complète
------------------------------------

Factorisation partielle
+++++++++++++++++++++++

Notons :math:`a_{i,j}` le coefficient :math:`(i,j)` de la matrice :math:`A`. Nous allons tout d'abord faire une **factorisation partielle** de la matrice

.. math::
  :label:eq-factorisation-partielle

  A=\begin{pmatrix}
    a_{0,0} & A_{0,1} \\
    A_{1,0}  & A_{1,1}
  \end{pmatrix} =
    \begin{pmatrix}
      1 & 0 \\
      L_{1,0} & I
    \end{pmatrix}
    \begin{pmatrix}
      u_{0,0} & U_{0,1} \\
      0 & S_{1,1}
    \end{pmatrix}

où :math:`I` est la matrice identité, les :math:`A_{I,J}` sont des sous-blocs de :math:`A` (notons que :math:`A_{0,0} = a_{0,0}` est un coefficient). Le bloc :math:`S_{1,1}=A_{1,1}-A_{1,0}A_{0,0}^{-1}A_{0,1}` est appelé le complément de Schur.

.. proof:exercise::

  Vérifiez que :

  - :math:`L_{1,0} = U_{0,0}^{-1}A_{1,0} = A_{1,0} / u_{0,0}`
  - :math:`U_{0,1} = A_{0,1}`
  - :math:`S_{1,1}=A_{1,1}-A_{1,0}A_{0,0}^{-1}A_{0,1} = A_{1,1} - L_{1,0}U_{0,1}`


.. proof:remark::

  Afin d'éviter toute confusion, nous utilisons des lettres et des indices minuscules pour les coefficients (*e.g.* :math:`a_{i,j}`) et des lettres et indices majuscules pour les blocs (*e.g* :math:`A_{I,J}`).


.. proof:remark::

  La factorisation partielle peut aussi être opérérée par bloc :

  .. math::

    A=\begin{pmatrix}
      A_{0,0} & A_{0,1} \\
      A_{1,0}  & A_{1,1}
    \end{pmatrix} =
      \begin{pmatrix}
        I & 0 \\
        L_{1,0} & I
      \end{pmatrix}
      \begin{pmatrix}
        U_{0,0} & U_{0,1} \\
        0 & S_{1,1}
      \end{pmatrix}


Factorisation complète
++++++++++++++++++++++

Le lien entre factorisation partielle et factorisation complète est donné par le théorème suivant :

.. proof:theorem::

  La matrice :math:`A` admet une factorisation :math:`LU` si et seulement si le bloc :math:`A_{0,0}` et le complément de Schur :math:`S_{1,1}` sont eux-mêmes factorisables. La décomposition :math:`LU` de la matrice est déterminée par les factorisations des blocs :math:`A_{0,0}=L_{0,0}U_{0,0} (=u_{0,0})` et :math:`S_{1,1} = L_{1,1}U_{1,1}` selon la formule :
  
  .. math::

      \begin{pmatrix}
        A_{0,0} & A_{0,1} \\
        A_{1,0}  & A_{1,1}
      \end{pmatrix}=
      \begin{pmatrix}
        L_{0,0} & 0 \\
        L_{1,0} & L_{1,1}
      \end{pmatrix}
      \begin{pmatrix}
        U_{0,0} & U_{0,1} \\
        0 & U_{1,1}
      \end{pmatrix}

    où :math:`L_{1,0}` et :math:`U_{0,1}` sont ceux de la **factorisation partielle** :eq:`eq-factorisation-partielle`.


Ce théorème nous dit que dès lors qu'on arrive à décomposer un bloc de la diagonale :math:`A_{0,0}` sous forme :math:`LU`, nous n'avons plus qu'à calculer :math:`L_{1,0}`, :math:`U_{0,1}` et :math:`S_{1,1}` puis on cherche la décomposition :math:`LU` de :math:`S_{1,1}`. Autrement dit, si nous disposons d'une fonction permettant de réaliser une **factorisation partielle** d'une matrice donnée, nous pouvons envisager un algorithme itératif pour obtenir la **factorisation complète** de la matrice.

Algorithme
----------

Principe
++++++++

Pour obtenir la factorisation complète, un algorithme itératif possible consiste à appliquer la factorisation partiellement successivement sur les compléments de Schur :math:`S_{k,k}` :

.. math::

  A = L^{(0)} U^{(0)}= \ldots = L^{(k)} U^{(k)} = \ldots = L^{(N-1)} U^{(N-1)}.

où les matrices :math:`L^{(k)}` et :math:`U^{(k)}` sont obtenues à la :math:`k^{\text{ème}}` itération. La petite animation suivante montre la forme de ces matrices dans le cas d'une taille N=5 :


.. raw:: html

  .. < div class="course_lu carousel_lu"  style="text-align:center">
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_0.svg" >
  .. < /div >
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_1.svg" >
  .. < /div >
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_2.svg" >
  .. < /div >
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_3.svg" >
  .. < /div >
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_4.svg" >
  .. < /div >
  .. < div class="course_lu carousel-cell_lu">
  .. < svg file="tps/img/lu_algo_5.svg" >
  .. < /div >
  .. < /div >

Pseudo code
+++++++++++

.. code-block::

  L = 0;
  U = 0;
  S = A;
  for k =0:N-1
    // Pivot
    pivot = S(k,k)
    // Colonne de L
    L(k,k) = 1;
    for i = k+1:N-1
      L(i,k) = S(i,k) / pivot;
    // Ligne de U
    U(k,k) = S(k,k);
    for j = k+1:N-1
      U(k,j) = S(k,j);
    // Complément de Schur
    for i = k+1:N-1
      for j = k+1:N-1
        S(i,j) = S(i,j) - L(i,k)*U(k,j);

Factorisation *sur place*
+++++++++++++++++++++++++

Plutôt que de stocker 3 matrices :code:`L`, :code:`U` et :code:`S`, dont :ref:`on sait que cela coûte très cher <sec-cpu-cost>`, on remarque que l'on peut se passer de ...:

- ... la matrice :code:`S` en modifiant directement :code:`U` : le bloc :math:`U_{k,k}` (en "bas à droite") contiendra le complément de Schur
- ... la matrice :code:`L` en la stockant dans :code:`U` et en supprimant son terme diagonal (qui vaut 1 et peut donc devenir "implicite")
- ... la matrice :code:`U` et travailler directement dans :code:`A` 

Cela donne le pseudo-code suivant :

{{% div class="course_lu carousel" %}}{{% div class="course_lu carousel-cell" style="width:50%; margin-right: 10px;"%}}

.. code-block::

  S = A;
  L = 0;
  U = 0;
  for k =0:N-1
    // Pivot
    pivot = S(k,k)
    // Colonne de L
    L(k,k) = 1;
    for i = k+1:N-1
      L(i,k) = S(i,k) / pivot;
    // Ligne de U
    U(k,k) = S(k,k);
    for j = k+1:N-1
      U(k,j) = S(k,j);
    // Complément de Schur
    for i = k+1:N-1
      for j = k+1:N-1
        S(i,j) = S(i,j) - L(i,k)*U(k,j);
```
Origine{{% /div %}}{{% div class="course_lu carousel-cell" style="width:50%; margin-right: 10px;"%}}
```

L = 0;
U = A;
for k =0:N-1
  // Pivot
  pivot = U(k,k)
  // Colonne de L
  L(k,k) = 1;
  for i = k+1:N-1
    L(i,k) = U(i,k) / pivot;
  // Ligne de U
  U(k,k) = U(k,k);
  for j = k+1:N-1
    U(k,j) = U(k,j);
  // Complément de Schur
  for i = k+1:N-1
    for j = k+1:N-1
      U(i,j) = U(i,j) - L(i,k)*U(k,j);
```
Suppression de S (stockée dans U){{% /div %}}{{% div class="course_lu carousel-cell" style="width:50%; margin-right: 10px;"%}}
```


U = A;
for k =0:N-1
  // Pivot
  pivot = U(k,k)
  // Colonne de L
  // U(k,k) = 1;
  for i = k+1:N-1
    U(i,k) = U(i,k) / pivot;
  // Ligne de U
  U(k,k) = U(k,k);
  for j = k+1:N-1
    U(k,j) = U(k,j);
  // Complément de Schur
  for i = k+1:N-1
    for j = k+1:N-1
      U(i,j) = U(i,j) - U(i,k)*U(k,j);
```
Suppression de L (stockée dans U){{% /div %}}{{% div class="course_lu carousel-cell" style="width:50%; margin-right: 10px;"%}}
```



for k =0:N-1
  // Pivot
  pivot = A(k,k)
  // Colonne de L
  // A(k,k) = 1;
  for i = k+1:N-1
    A(i,k) = A(i,k) / pivot;
  // Ligne de U
  A(k,k) = A(k,k);
  for j = k+1:N-1
    A(k,j) = A(k,j);
  // Complément de Schur
  for i = k+1:N-1
    for j = k+1:N-1
      A(i,j) -= A(i,k)*A(k,j);
```
Suppression de U{{% /div %}}{{% /div %}}


## Implémentation en C++

{{% callout exercise %}}
Avant de coder quoi que ce soit, modifiez le **pseudo code** de la factorisation `LU` de `A` effectuée **directement dans la matrice** `A` : nettoyez le de certaines opérations rendues inutiles !
{{% /callout %}}

{{% callout exercise %}}
Implémentez une méthode de la classe `Matrice` qui factorise la `Matrice` *sur place* :
```cpp
void Matrice::decomp_LU();
```
{{% /callout %}}

{{% callout warning %}}
Après application de l'algorithme, la `Matrice A` sera modifiée de telle sorte que sa partie triangulaire inférieure soit égale à :math:`L` (sans la diagonale unitaire), et sa partie triangulaire supérieure sera égale à :math:`U` (diagonale incluse). Cette méthode permet de diminuer le coût mémoire de stockage mais, attention :

- Le **produit matrice vecteur n'a alors plus de sens** une fois cet algorithme appliqué !
- Il ne faut pas ré-appliquer la factorisation LU sur A (c'est de toute façon inutile, mais une erreur arrive si vite...)


Il peut être intéressant de rajouter un paramètre à la classe `Matrice` de type `booleen` (un "flag") permettant de déterminer si une matrice a été, ou non, déjà factorisée.
{{% /callout %}}


## Validation

{{% callout tips %}}
Une première étape pour valider votre factorisation LU : calculer le produit :math:`LU`, vous devez retrouver :math:`A` !
{{% /callout %}}

{{% callout exercise %}}
Validez votre factorisation :math:`LU` sur la matrice suivante :
$$
A = \begin{pmatrix}
  2 & -1 & 0 & 0 &0\\
  -1 & 2 & -1 & 0 &0\\
  0 & -1 & 2 & -1 &0\\
  0 & 0& -1 & 2 & -1 \\
  0 & 0& 0 &-1 & 2 \\
\end{pmatrix},
$$
dont les matrices :math:`L` et :math:`U` sont données par :
$$
\underbrace{\begin{pmatrix}
  1 & 0 & 0 & 0 &0\\
  -0.5 & 1 & 0 & 0 &0\\
  0 & -\frac{2}{3} & 1 & 0 &0\\
  0 & 0 & -0.75 & 1 & 0 \\
  0 & 0 & 0 &-0.8 & 1 
\end{pmatrix}}_{L}
\underbrace{\begin{pmatrix}
  2 & -1 & 0 & 0 &0\\
  0 & 1.5 & -1 & 0 &0\\
  0 & 0 & \frac{4}{3} & -1 &0\\
  0 & 0& 0 & 1.25 & -1 \\
  0 & 0& 0 &0 & 1.2 \\
\end{pmatrix}}_{U}
$$
Notez que cette matrice fait partie [des matrices de test régulières]({{<relref "../dense/test-matrices.md">}}).
{{% /callout %}}

{{% callout exercise %}}
Résolvez numériquement le problème suivant à l'aide de la factorisation LU :
$$
A X= b,
$$
où :math:`A` est la matrice de l'exercice précédent et :math:`b = [1,1,1,1,1]^T`. La solution du problème est :math:`X = [2.5, 4,4.5, 4,2.5]^T`.
{{% /callout %}}


{{% js type="text/javascript" src="https://npmcdn.com/flickity@2/dist/flickity.pkgd.js" %}}
{{% js type="text/javascript" src="../js/lu_fact.js" %}}
