Gradient Conjugué
=================

Objectifs
---------

- Implémenter la méthode du Gradient Conjugué
- Ajouter un éventuel préconditionneur
- Comparer les performances


Méthode du Gradient Conjugué
----------------------------

Pseudo-code
+++++++++++

Remarque : :code:`(z,r)` représente le produit scalaire en :code:`z` et :code:`r`.

.. code-block::

  x_0 = 0
  r_0 = b - A*x_0
  p_0 = r_0
  k = 0
  while (|r| / |b| > tolerance && k < nmax)
    k++
    alpha_k = |r_k|²/(p_k, A*p_k)
    x_{k+1} = x_k + alpha_k * p_k
    r_{k+1} = r_k - alpha_k*(A*p_k)
    beta_k  = |r_{k+1}|²/|r_k|²
    p_{k+1} = r_{k+1} + beta_k*p_k
  end
  return x_{k+1}


Implémentation
--------------

Implémentez la méthode du Gradient Conjugué. Validez ensuite votre code la sur la :ref:`matrice du Laplacien <sec-test-matrices>`.

Comparaison  des performances
-----------------------------

Entre solveurs
++++++++++++++

.. proof:exercise::

  Pour une tolérance de 0.1, une taille de matrice N = 1000 et un nombre maximal d'itérations égal à N, affichez les historiques de convergence du gradient conjugué et des autres méthodes itératives. Comparez également le temps CPU entre les différentes méthodes.

Vous devriez obtenir des résultats similaires à ceux ci-dessous (sauf pour le temps CPU !) :

.. raw:: html

  TODO: <div id="convergence_history"></div>

  {{% div id="table_cg"%}}

+---------------------+----+----+----+----+----+
| Méthode             |    |    |    |    |    |
+=====================+====+====+====+====+====+
| Nombre d'itérations |    |    |    |    |    |
+---------------------+----+----+----+----+----+
| Temps CPU (s)       |    |    |    |    |    |
+---------------------+----+----+----+----+----+

.. raw:: html
  
  {{% /div %}}


En fonction de la taille de la matrice
++++++++++++++++++++++++++++++++++++++


.. proof:exercise::

  Pour une tolérance de 0.001 et une taille N = 100, 200, ..., 1000, affichez le nombre d'itérations requises par le gradient conjugué ainsi que le temps CPU.


Vous devriez obtenir des résultats proches de ceux-ci (sauf pour le temps CPU) :

+---------------------+--------+-------+-------+-------+-------+-------+--------+--------+--------+--------+
| N                   | 100    | 200   | 300   | 400   | 500   | 600   | 700    | 800    | 900    | 1000   |
+=====================+========+=======+=======+=======+=======+=======+========+========+========+========+
| Nombre d'itérations | 50     | 100   | 150   | 200   | 250   | 300   | 350    | 400    | 450    | 500    |
+---------------------+--------+-------+-------+-------+-------+-------+--------+--------+--------+--------+
| Temps CPU (s)       | 0.0534 | 0.178 | 0.613 | 1.429 | 3.303 | 7.485 | 11.771 | 15.939 | 24.415 | 34.409 |
+---------------------+--------+-------+-------+-------+-------+-------+--------+--------+--------+--------+


.. raw:: html
  
  TODO:
  <script type="text/javascript" src="../js/data_cg.js" />
  <script type="text/javascript" src="../js/cg.js" />

