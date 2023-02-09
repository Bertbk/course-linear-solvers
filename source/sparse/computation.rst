Implémentation des Matrices Creuses
===================================

Objectifs
---------

Implémenter les matrices creuses au format COO et CSR


.. proof:warning::

  Ne vous aventurez pas ici sans avoir lu (et compris) la :ref:`section dédiée aux formats COO et CSR <sec-format-coo-csr>` !


.. proof:remark::

  Une classe pour les gouverner toutes ~~et dans les ténèbres les lier~~ ? Vous pouvez utiliser `l'héritage en C++ <https://openclassrooms.com/fr/courses/1894236-programmez-avec-le-langage-c/1898475-decouvrez-lheritage>`_ afin que chaque classe matricielle (dense, COO, CSR) héritent d'une classe mère abstraite. Cependant, pour simplifier, nous proposons, dans un premier temps au moins, de constuire deux classes :code:`MatriceCOO` et :code:`MatriceCSR` distinctes et indépendantes.


.. proof:warning::

  Rappelons que **notre** implémentation du COO **n'autorise pas** les doublons !


La figure suivante rappelle l'agencement de notre code :


.. figure:: /img/sparse/coo_to_csr.*
  :figwidth: 100%
  :width: 100%
  :alt: Construction de matrices COO à l'aide de Triplet et convertisseur en CSR
  :align: center

  Construction de matrices COO à l'aide de Triplet et convertisseur en CSR


Classe :code:`Triplet`
----------------------

Un :code:`Triplet` est défini par 3 valeurs : les indices ligne :code:`i` et colonne :code:`j` ainsi que la valeur du coefficient :code:`val`. Cette classe devra également comporter des opérations de comparaison pour pouvoir trier facilement le vecteur de :code:`Triplet` à l'aide de la méthode :code:`sort` de :code:`std::vector`. Nous définissons ainsi la comparaison entre deux :code:`Triplet` :


.. code-block:: cpp

  class Triplet{
    [...]
  };
  // Surchage des opérations de comparaison
  bool operator<(const Triplet &S, const Triplet &T);
  bool operator>(const Triplet &S, const Triplet &T);
  bool operator==(const Triplet &S, const Triplet &T);

.. proof:theorem::

  Soient deux triplets S et T. Nous avons alors :

  - :math:`\texttt{S} > \texttt{T}` si et seulement si
    
    .. math::

      \texttt{S.i} > \texttt{T.i} \text{ ou } (\texttt{S.i} == \texttt{T.i} \text{ et } \texttt{S.j} > \texttt{T.j})
  
  - :math:`\texttt{S} < \texttt{T}` si et seulement si

    .. math::

      \texttt{S.i} < \texttt{T.i} \text{ ou } (\texttt{S.i} == \texttt{T.i} \text{ et } \texttt{S.j} < \texttt{T.j})
  
  - :math:`\texttt{S} == \texttt{T}` si et seulement si
  
    .. math::

      \texttt{S.i} == \texttt{T.i} \text{ et } \texttt{S.j} == \texttt{T.j}
  

Autrement dit, soit les indices lignes sont différents et la comparaison est immédiate, soit les indices lignes sont identiques et on compare les indices colonnes. En cas d'égalité, nous dirons que les Triplets sont identiques, peu importe la valeur de :code:`val`. Cependant, rappelons que ce cas posera des problèmes pour notre implémentation car nous n'autorisons pas les doublons !

.. proof:exercise::

  Implémentez la classe :code:`Triplet` ainsi que les opérateurs de comparaisons. 

.. proof:warning::

  Les opérateurs :code:`<` et :code:`>` ne sont pas opposés : si S < T alors on n'a pas forcément S > T (ils peuvent être égaux) !


Classe :code:`MatriceCOO`
-------------------

Une matrice COO comporte un tableau de :code:`Triplet` (:code:`std::vector<Triplet>`). La méthode qui nous intéresse est :code:`MatriceCOO:to_csr()` qui retourne une matrice au format CSR. 

Nous vous conseillons ici d'implémenter les classes :code:`Triplet` et :code:`MatriceCOO`. Laissez pour le moment la méthode :code:`MatriceCSR MatriceCOO::to_csr()`  de côté mais validez tout le reste (constructeurs, destructeurs, affichage, ...) !

Pensez également à ajouter une méthode ou une fonction permettant de construire facilement :ref:`la matrice du Laplacien <sec-test-matrices>`.

.. proof:remark::

  Pour afficher une matrice creuse (COO ou CSR), vous pouvez :

  - Afficher les Triplet (ou les tableau :code:`row`, :code:`col` et :code:`val`)
  - Transformer la matrice au format dense et afficher cette dernière. Cette méthode est clairement peu efficace mais afficher une matrice n'a pas vocation à être performant : c'est utilisé surtout pour débugguer !

.. proof:remark::

  N'oubliez pas, dans les paramètres de vos matrices creuses, la taille de celle-ci et pourquoi pas le nombre de non-zéros :code:`nnz`.


Classe :code:`MatriceCSR`
-------------------------

Principalement, une matrice CSR comporte trois tableaux : :code:`row`, :code:`col` et :code:`val`. 

Implémentez une classe :code:`MatriceCSR` avec notamment et surtout une **surcharge de `operator*` pour le produit Matrice-Vecteur**. Le produit Matrice-Matrice est plus compliqué car on ne connait pas la forme a priori de la Matrice CSR ainsi obtenue.

Vous aurez aussi certainement besoin d'un constructeur prenant trois tableaux :code:`row`, :code:`col` et :code:`val` et les recopiant dans une nouvelle `MatriceCSR`.

Validez naturellement votre code avant de passer à la suite ! Pour vous aider dans cette étape, vous pouvez reprendre la :ref:`matrice précédente <format-csr-principe>` et l'exemple de résultat suivant :

.. math::

  \begin{pmatrix}
    3 & 0 & 0 & 2 & 1 \\
    0 & 0 & 5 & 8 & 0 \\
    0 & 1 & 2 & 0 & 0 \\
    0 & 0 & 9 & 0 & 0 \\
    0 & 0 & 10& 4 & 0
  \end{pmatrix}
  \begin{pmatrix}
  1.5\\ 2.5\\ 3.5\\ 4.5\\ 5.5
  \end{pmatrix}=
  \begin{pmatrix}
  19\\ 53.5\\ 9.5\\ 31.5\\ 53
  \end{pmatrix}


De COO à CSR : :code:`to_csr()`
-------------------------------

Nous disposons maintenant de tous les outils pour implémenter la méthode permettant de construire une matrice CSR à partir d'une matrice COO. Nous supposons que la :code:`MatriceCOO` est construite et dispose d'un tableau de :code:`Triplet`. Les deux étapes pour générer une matrice CSR sont :

1. Trier le tableau de :code:`Triplet` par indices de ligne croissants puis indices de colone croissants
2. Extraire les trois tableaux :code:`row`, :code:`col` et :code:`val` du tableau de :code:`Triplet`
3. Compresser le tableau :code:`row`

Pour gagner en rapidité, l'étape de tri peut se faire à l'aide de la fonction :code:`std::sort()` de la bibliothèque :code:`algorithm`. Nous avons implémenter les opérateurs d'ordre :code:`>`, :code:`<` et :code:`==` ce qui permet, en une ligne :

.. code-block:: cpp

  std::vector<Triplet> triplets_;
  [...]
  std::sort(triplets_.begin(), triplets_.end());


Les étapes 2 et 3 peuvent être réalisés en même temps durant le parcours du tableau de :code:`Triplet`.
