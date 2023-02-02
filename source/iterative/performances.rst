Comparaison des Performances
============================

Objectifs
---------

Pour les différentes méthodes itératives standards, nous souhaitons comparer :

1. Les **historiques de convergence**
2. Le **temps CPU** (*time to solution*)

.. proof:tips::

  Pensez à préparer vos classes/fonctions pour :ref:`sortir et traiter vos données <sec-json>`.

Problème modèle
---------------

Nous utilisons toujours :ref:`la matrice du Laplacien <sec-test-matrices>`, de taille N⨉N.

Temps CPU
---------

Adaptez les fonctions membres :code:`Solve()` de chaque classe de méthode itérative pour pouvoir calculer le temps d'exécution de la résolution. **Vous pouvez bien entendu ajouter des paramètres/méthodes si vous le désirez**.

Naturellement, vous pouvez réutiliser :ref:`le code minimaliste proposé dans ces tps <sec-cpu-time>`.

Historiques de Convergence
--------------------------

Nous considérons une matrice de taille 200 et un vecteur membre de droite :code:`b` rempli de 1. Dans cet exercice, nous fixons de plus la tolérance à 0.1 et le nombre d'itérations maximal de 20000.

.. proof:exercise::

  Sur une même figure, affichez les courbes de la norme du résidu relatif (:math:`\|\mathrm{r}\|/\|\mathrm{b}\|) en fonction du numéro de l'itération pour chaque méthode itérative. Cette figure s'appelle **l'historique de convergence**.

  Quelle méthode itérative est la plus rapide (en terme de nombre d'itérations) ?


Vous devriez obtenir une courbe ressemblant à celle ci-dessous :

.. raw:: html

  TODO: <div id="convergence_history"></div>

Avec les résultats suivants (**le temps CPU dépend bien évidemment de l'ordinateur** et de l'implémentation !) :

.. raw:: html

  TODO: {{% div id="table_iterative_standard" %}}

+---------------------+--------+--------------+----------------------+
| Méthode             | Jacobi | Gauss-Seidel | Relaxation (optimal) |
+=====================+========+==============+======================+
| Nombre d'itérations |        |              |                      |
+---------------------+--------+--------------+----------------------+
| Temps CPU (s)       |        |              |                      |
+---------------------+--------+--------------+----------------------+

.. raw:: html

  TODO: {{% /div %}}

Temps CPU
---------

.. proof:exercise::

  Pour N=10 à 200, avec un pas de 10, calculez le temps CPU (en secondes) pour chaque méthode itérative. Affichez sur une même figure chaque courbe :ref:`"temps CPU (s)" <sec-cpu-time>` en fonction du "numéro de l'itération".

  Quelle méthode itérative est la plus rapide (en terme de secondes) ?

.. raw:: html

  TODO:
  {{% js type="text/javascript" src="../js/data_standard_iterative.js"%}}
  {{% js type="text/javascript" src="../js/standard_iterative.js"%}}