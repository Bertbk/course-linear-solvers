.. _sec-triangular-system:

Systèmes Triangulaires
======================

Objectif
--------

Résoudre un système linéaire triangulaire inférieur ou supérieur.

Préparation
-----------

Il est judicieux de continuer à travailler dans les mêmes fichiers que :ref:`ceux utilisés pour les systèmes diagonaux <sec-diag-system>` et d'ajouter à la suite vos nouvelles fonctions.

Problème
--------


Voici la forme globale de matrices triangulaires inférieures et supérieures  :

.. math::

  \underbrace{\begin{pmatrix}
    a & 0 & 0& 0 & 0\\
    b & c & 0& 0 & 0\\
    d & e & f& 0 & 0\\
    g & h & i& j & 0\\
    k & l & m& n & p
  \end{pmatrix}}_{\text{Triang. inf.}}\quad\text{et}\quad
  \underbrace{\begin{pmatrix}
    a & b & c& d & e\\
    0 & f & g& h & i\\
    0 & 0 & j& k & l\\
    0 & 0 & 0& m & n\\
    0 & 0 & 0& 0 & p
  \end{pmatrix}}_{\text{Triang. sup.}}.

Nous considérons un système linéaire :math:`AX = b` où la matrice :math:`A` est triangulaire. Pour résoudre ce problème, nous n'inversons pas la matrice :math:`A` car cette opération est coûteuse et surtout, nous pouvons obtenir un algorithme performant qui, connaissant :math:`b` et :math:`A`, permet d'obtenir le vecteur :math:`X`.

.. proof:remark::

  Rappel : Nous cherchons $X$ et non $A^{-1}$ !

Algorithme de résolution
------------------------

.. proof:exercise::

  Écrivez (sur papier) l'algorithme permettant de résoudre le système linéaire quand la matrice est triangulaire inférieure. 

  Par un raisonnement similaire, obtenez l'algorithme de résolution pour une matrice triangulaire supérieure.

.. proof:warning::

  Rappel : Ces algorithmes ne doivent en aucun cas calculer la matrice :math:`A^{-1}` mais uniquement le vecteur :math:`A^{-1}b` !

.. proof:tips::

  Pour obtenir et comprendre ces algorithmes, n'hésitez pas à travailler sur une "vraie" matrice, mais soyez humble et commencez petit avec une matrice 3x3 par exemple.



Implémentation
--------------

Triangulaire Inférieure
+++++++++++++++++++++++

Implémentez une fonction qui prend en argument une :code:`Matrice A`, **supposée triangulaire inférieure**, un :code:`Vecteur b` et qui renvoie le résultat :code:`Vecteur X` tel que :code:`AX=b` :

.. code-block:: cpp

  Vecteur solve_triang_inf(const Matrice &A, const Vecteur& b);

.. proof:warning::

  **Ne vérifiez pas si la matrice est bien triangulaire** ou non. En plus de coûter cher, une telle vérification est inutile voire nuisible pour la suite.

Triangulaire Supérieure
-----------------------

Pour une matrice triangulaire supérieure, programmez une fonction similaire en étant prudent(e) quant au sens de parcours de la :code:`Matrice A` :

.. code-block:: cpp

  Vecteur solve_triang_sup(const Matrice &A, const Vecteur& b);


Cas particulier : Diagonale = 1
-------------------------------

Afin de faciliter ce qui suivra, construisez les deux autres fonctions suivantes :

.. code-block:: cpp

  Vecteur solve_triang_inf_id(const Matrice &A, const Vecteur& b);
  Vecteur solve_triang_sup_id(const Matrice &A, const Vecteur& b);

Ces fonctions résolvent respectivement un système linéaire triangulaire inférieur et supérieur et tel **que la diagonale de la** :code:`Matrice A` **est composée uniquement de 1**. Comme précédemment, nous ne vérifierons pas une telle propriété mais la supposerons vraie.
