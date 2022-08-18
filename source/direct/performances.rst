Comparaison des Performances
============================

Objectifs
---------

1. Retrouver les temps de complexité pour la LU et Cholesky
2. Comparer le temps d'exécution entre les factorisations de Cholesky et LU

Préparation des classes/fonctions/méthodes
------------------------------------------

Temps CPU
+++++++++

Nous baserons nos tests sur le temps CPU mis par chaque méthode/fonction de factorisation. Pour cela, modifiez les afins de pouvoir calculer ce temps CPU, :ref:`en vous aidant si vous le souhaitez du code minimaliste <sec-cpu-time>`.

Format de sortie et visualisation
+++++++++++++++++++++++++++++++++

Nosu vous invitons aussi à préparer/modifier vos classes/fonctions/méthodes pour :ref:`sortir et traiter vos données <sec-json>`. Vous pouvez par exemple ajouter des paramètres :code:`double cpu_lu_` et :code:`double cpu_cholesky_` à votre classe :code:`Matrice` ou calculer directement le temps CPU des factorisations dans la fonction :code:`main` au moment des appels à celles-ci.

Matrice Test : Laplacien
------------------------

Nous baserons nos tests sur la `matrice du Laplacien <sec-test-matrices>` $A_N$.


.. proof:exercise::

  Pour N=50, 100, 150, ... (ou autre, à vous de voir!) :

  1. Calculez les temps d'exécution pour factoriser chacune des deux méthodes
  2. Dessinez les graphes du temps d'exécution par rapport à la taille de la matrice. Placez-vous en échelle logarithmique sur l'axe des abscisse afin de retrouver la complexité des algorithmes ($N^3$ pour les deux).

