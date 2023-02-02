Préconditionnement
==================

Objectifs
---------

- Ajouter un préconditionneur
- Comparer les performances

Principe 
--------

Le nombre d'itérations nécessaire pour la méthode du Gradient Conjugué peut être diminué. Nous savons que la convergence de la méthode  dépendant du conditionnement de la matrice du système linéaire. Or, pour la matrice du Laplacien, ce conditionnement tend vers l'infini quand la taille :math:`N` du système tend également vers l'infini. Cela nous amène à étudier la notion de préconditionnement.

Prenons un système linéaire :math:`Ax = b`. Préconditionner ce système revient à multiplier le système par une matrice :math:`P^{-1}` et à résoudre le système linéaire suivant:

.. math::

  P^{-1}Ax = P^{-1}b.

Le vecteur :math:`x` est également solution du système original. La matrice :math:`P` doit être choisie de telle sorte que :

1. Elle soit "facilement" inversible, au sens où le coût de calcul de :math:`P^{-1}` doit rester faible
2. Le conditionnement de la matrice :math:`P^{-1}A` soit le plus proche possible de 1 (:math:`P^{-1}A` doit être proche de l'identité).
3. Si l'on souhaite utiliser la méthode du Gradient Conjugué pour résoudre le nouveau système, :math:`P^{-1}A` soit toujours symétrique et définie positive.

Notez que le meilleur préconditionneur possible, au sens du critère 2, est bien entendu :math:`P^{-1} = A^{-1}`, mais cela ne respecterait pas le critère 1.

Préconditionneur simple : Jacobi
--------------------------------

Un préconditionneur très simple est celui de Jacobi, :math:`P_J := \text{diag}(A)`. Dans le cas d'une matrice à coefficients diagonaux identiques, comme la :ref:`matrice du Laplacien <sec-test-matrices>`, ce préconditionneur n'a aucune influence sur la convergence. 

Matrice test
------------

Nous proposons de travailler sur une autre matrice symétrique définie positive :math:`H_n`, de taille :math:`n\times n`, telle que les seuls coefficients non nuls sont donnés par :

.. math::

  \forall i = 0,\ldots, n-1, \qquad
  \left\{
    \begin{array}{r c l l}
      H_n(i,i) &=& i+1,\\
      H_n(i,i+2) &=& 1 & \text{ si } i+2 \leq n\\
      H_n(i,i-2) &=& 1 & \text{ si } i-2 \geq 0
    \end{array}
  \right.


.. proof:exercise::

  Comme pour la :ref:`matrice du Laplacien <sec-test-matrices>`, implémentez une méthode de la classe :code:`Matrice` permettant d'obtenir la matrice :math:`H_n` très rapidement.

Gradient Conjugué Précondtionné
-------------------------------

Pseudo-code
+++++++++++

Le gradient conjugué préconditionné est modifié ainsi (:code:`(z,r)` représente le produit scalaire en :code:`z` et :code:`r`)

.. code-block:: cpp

  x_0 = 0
  r_0 = b - A*x_0
  z_0 = P^{-1}*r0
  p_0 = z_0
  k = 0
  while (|r| / |b| > tolerance && k < nmax)
    k++
    alpha_k = (r_k, zk)/(p_k, A*p_k)
    x_{k+1} = x_k + alpha_k * p_k
    r_{k+1} = r_k - alpha_k*(A*p_k)
    z_{k+1} = P^{-1}*r_{k+1}
    beta_k  = (z_{k+1}, r_{k+1})/(z_k,r_k)
    p_{k+1} = z_{k+1} + beta_k*p_k
  end
  return x_{k+1}


Historique de convergence
+++++++++++++++++++++++++

Soit :math:`b_n le vecteur de taille :math:`n` rempli de 1, nous considérons le problème linéaire suivant

.. math::
  :label: eq-hnsystem

  H_n x = b_n,

et sa version préconditionnée par Jacobi (où :math:`P_n = \text{diag}(H_n)`)

.. math::
  :label: eq-pnhnsystem

  P_n^{-1}H_n x = P_n^{-1}b_n.

.. proof:exercise::
  
  Pour une tolérance de 0.01 et une taille N = 1000, résolvez les système \eqref{eq:hnsystem} ainsi que sa variante préconditionnée :eq:`eq-pnhnsystem` à l'aide des méthodes itératives standards et du gradient conjugué. 

  Comparez le nombre d'itérations pour chaque méthodes et affichez, sur une même courbe, l'historique de convergence de chaque méthode.


.. proof:remark::

  Pour les méthodes itératives standards, vous devez calculer la matrice :math:`P_n^{-1}H_n`, cependant pour le gradient conjugué, vous devez utiliser la version préconditionnée de l'algorithme.

