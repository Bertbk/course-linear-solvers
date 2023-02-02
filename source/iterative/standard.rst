Méthodes Itératives Stationnaires Standards
===========================================

Objectifs
---------

1. Implémenter toutes les méthodes itératives standards
2. Comparer la vitesse de convergence des différentes méthodes


.. _sec-generic-algo:

Algorithme générique
--------------------

Pour résoudre le problème linéaire :math:`A x= b`, une technique consiste à réécrire la matrice :math:`A` sous la forme $A = M - (M-A)$, où :math:`M` est une matrice bien choisie,  et d'appliquer le schéma itératif suivant

.. math::
  :label: eq-split

  Mx_{n+1} = (M-A)x_n + b

à convergence, nous retrouvons bien :math:`(M-(M-A))x = Ax = b`.

.. proof:exercise::

  En introduisant le résidu :math:`r_n := b - Ax_n`, montrez que l'équation :eq:`eq-split` se réécrit :

  .. math::

    x_{n+1} = x_n + M^{-1}r_n.


L'algorithme de résolution décrit par l'équation :eq:`eq-split` s'écrit alors, pour une tolérance :math:`\varepsilon` et un nombre maximal d'itérations :math:`n_{max}` donnés :

.. code-block:: cpp

  niter = 0           // nombre d'itérations
  x = 0               // vecteur solution
  r = b               // vecteur résidu
  resvec[0] = |r|/|b| // Résidu relatif de l'itération 0
  while( (|r|/|b| > tolerance) && (niter < n_max))
    y = M^{-1}*r            // Calcul de la direction (résolution système)
    x = x + y               // Mise à jour de la solution courante
    r = b - Ax              // Mise à jour du résidu (vecteur)
    niter = niter + 1       // Mise à jour du numéro de l'itération
    resvec[niter] = |r|/|b| // Stockage du résidu relatif (à des fins d'analyse)
  end

Les méthodes itératives que nous détaillons ici ne diffèrent que par le choix de la matrice :math:`M`. Pour chacune d'entres elles, nous construirons une classe dédiée, bien qu'elles partagent toutes des codes similaires (nous pourrions utiliser l'héritage, mais cela compliquera la "templatisation").

Méthodes standards
------------------

La matrice :math:`M` est choisie selon le *splitting* (découpe) d'une matrice :math:`A` comme :math:`A = D - E - F`, où :math:`D`, :math:`E` et :math:`F` sont des matrices de la même taille que :math:`A` et telles que (attention au signe des coefficents de :math:`E` et :math:`F`) :

- :math:`D = {\rm diag}(A)` : Matrice ne contenant que **les termes diagonaux** de :math:`A`
- :math:`E` : Matrice ne contenant que la partie **triangulaire inférieure stricte** de :math:`-A`
- :math:`F` : Matrice ne contenant que la partie **triangulaire supérieure stricte** de :math:`-A`

.. figure:: /img/DEF.*
  :figwidth: 100%
  :width: 100%
  :alt: Splitting d'une matrice
  :align: center

  Splitting d'une matrice

Ensuite, selon les choix pour la matrice :math:`M`, la méthode obtenue sera différente :

+--------------+-------------------------------+-------------------------------------------------------------------------------------+
| Méthode      | Matrice :math:`M`             | Remarques                                                                           |
+==============+===============================+=====================================================================================+
| Jacobi       | :math:`D`                     | :math:`M^{-1} = D^{-1}` est explicite                                               |
+--------------+-------------------------------+-------------------------------------------------------------------------------------+
| Gauss-Seidel | :math:`D - E`                 | :math:`M` est triangulaire inférieure                                               |
+--------------+-------------------------------+-------------------------------------------------------------------------------------+
| Relaxation   | :math:`\frac{1}{\omega}D - E` | :math:`M` est triangulaire inférieure (:math:`0 < \omega < 2` paramètre à contrôler)|
+--------------+-------------------------------+-------------------------------------------------------------------------------------+


.. proof:warning::

  Cette décomposition :math:`A = D - E - F` **n'a aucun rapport** avec la factorisation :math:`LU` !


Une Classe en Détail : Jacobi
-----------------------------

Pour chaque méthode, nous proposons d'implémenter une classe distincte et donc, de créer deux fichiers (un en-tête et un source). Par exemple, pour Jacobi : :code:`include/jacobi.hpp` et :code:`src/jacobi.cpp`. Les méthodes se ressemblant, ces différentes classes se ressembleront naturellement. Nous détaillons ici la classe :code:`Jacobi`, cependant, pour Gauss-Seidel et la Relaxation, la classe sera similaire à quelques modifications près (par exemple la Relaxation nécessite un paramètre :math:`\omega` en plus).

Données Membres (ou paramètres)
+++++++++++++++++++++++++++++++

Celles-ci seront séparées en deux, les données "entrantes", fournies par l'utilisateur, et les données "sortantes", calculées lors de la résolution du problème linéaire. En pratique, **rien ne différencie ces deux types de données** qui sont de type :code:`private`, seule **leur utilisation** permet de les distinguer : 

1. Les données *entrantes* sont fournies par l'utilisateur et ne sont pas modifiées par la classe
2. Les données *sortantes* sont modifiées par la classe (vecteur solution, nombre d'itération, ...) et seront récupérées par l'utilisateur à des fins d'analyses

Nous proposons les paramètres suivants, mais vous pouvez bien entendu en ajouter !

+--------+-----------------------------+------------------+-------------------------------------------------------------+
| I/O    | Type                        | Nom (suggestion) | Fonction                                                    |
+========+=============================+==================+=============================================================+
| Entrée | :code:`const Matrice &`     | :code:`A_`       | Matrice (dense) du système.                                 |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Entrée | :code:`const Vecteur &`     | :code:`b_`       | Vecteur (membre de droite)                                  |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Entrée | :code:`double`              | :code:`tol_`     | Tolérance                                                   |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Entrée | :code:`int`                 | :code:`n_max_`   | Nombre maximum d'itérations                                 |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Sortie | :code:`Vecteur`             | :code:`x_`       | Vecteur solution                                            |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Sortie | :code:`int`                 | :code:`niter_`   | Nombre d'itérations                                         |
+--------+-----------------------------+------------------+-------------------------------------------------------------+
| Sortie | :code:`std::vector<double>` | :code:`resvec_`  | Tableau des normes des résidus relatifs (*RESidual VECtor*) |
+--------+-----------------------------+------------------+-------------------------------------------------------------+

.. proof:remark::

  Libre à vous d'ajouter d'autres paramètres, de ne pas utiliser ceux que l'on propose ou d'en changer le nom.

La figure suivante illustre ces paramètres et la classe `Jacobi` :

.. figure:: /img/jacobi.*
  :figwidth: 100%
  :width: 100%
  :alt: Illustration de la classe `Jacobi`
  :align: center

  Illustration de la classe `Jacobi`

Constructeurs
+++++++++++++

À vous de décider ce dont vous avez besoin :  un constructeur vide ? Un qui prend toutes les données d'entrée en argument, par exemple :

.. code-block::

  Jacobi(const Matrice &A, const Vecteur &b, double tol, int maxit);


.. proof:tips::

  Pour gagner en efficacité et limiter le coût mémoire, il est plus intéressant de ne pas stocker les Matrice et Vecteur, mais plutôt leur adresse (référence ou pointeur) d'où un paramètre de type `const Matrice &` et `const Vecteur &`. Cependant, attention, l'affectation de cette valeur doit s'effectuer *avant* l'entrée dans le constructeur à l'aide `d'une liste d'initialisation <https://openclassrooms.com/fr/courses/1894236-programmez-avec-le-langage-c/1897606-creez-les-classes-partie-2-2#/id/r-1907275>`_ :

  .. code-block:: cpp

    Jacobi(const Matrice & A, const Vecteur & b,...)
      :A_(A), b_(b)... // Liste d'initialisation
    {
        blabla
    }

  Notez que l'utilisation de listes d'initialisation est une très bonne pratique, y compris pour les autres paramètres.


Méthode :code:`Solve()`
+++++++++++++++++++++++

Outre les accesseurs (*getter*) et les mutateurs (*setter*) habituels pour respectivement accéder et modifier les paramètres, nous avons au moins besoin d'une fonction membre qui résout le système linéaire en appliquant :ref:`l'algorithme de résolution <sec-generic-algo>`. Celle-ci aura (probablement) le prototype suivant :

.. code-block:: cpp

  void Jacobi::Solve();

D'autre part, voici quelques propositions pour définir la méthode :

- La solution sera stockée dans :code:`x_`
- Le nombre d'itérations sera stocké dans :code:`niter_`
- À chaque itération, la norme du résidu relatif :math:`\frac{\|r\|}{\|b\|}` doit être calculé pour vérifier si la convergence est atteinte ou non. Cette quantité sera stockée dans le tableau :code:`resvec_` au fur et à mesure des itérations. Ceci nous sera utile pour la partie analyse.


.. proof:remark::

  Vous aurez certainement besoin de modifier la classe `Vecteur` pour ajouter des fonctionnalités comme par exemple, le calcul de sa norme.


.. proof:warning::

  Vous **ne devez surtout pas extraire la diagonale** de la matrice dans une nouvelle `Matrice` : si :math:`A` est une matrice prenant 4Go de mémoire, la duppliquer n'apparait pas comme une solution très rusée.

  Pour un vecteur :math:`x`, calculer :math:`(\text{diag}(A))^{-1}x` est très simple : il suffit de parcourir les éléments diagonales de :math:`A`. Vous pouvez créez **une nouvelle fonction résolvant un système diagonal**, dans la même lignée des fonctions utilisées pour un système triangulaire.


Implémentation et Test
++++++++++++++++++++++

Pour tester et valider votre code, utiliser la même matrice que pour la factorisation LU :

.. math::

  \begin{pmatrix}
    2 & -1 & 0 & 0 &0\\
    -1 & 2 & -1 & 0 &0\\
    0 & -1 & 2 & -1 &0\\
    0 & 0& -1 & 2 & -1 \\
    0 & 0& 0 &-1 & 2 \\
  \end{pmatrix} X=
  \begin{pmatrix}
    1 \\
    1 \\
    1 \\
    1 \\
    1 \\
  \end{pmatrix}.

La solution exacte de ce problème est :math:`X = [2.5, 4,4.5, 4,2.5]^T`.

.. proof:exercise::

  Construisez une telle classe :code:`Jacobi`. N'oubliez surtout pas de :

  1. Testez la compilation
  2. Testez le résultat de votre implémentation sur un cas petit et simple.
  3. Comparez la solution obtenue avec Jacobi avec celle obtenue par résolution directe.
  4. Tant que les trois points ci-dessus ne sont pas validés, ne passez pas à la suite !

Classe Gauss-Seidel
-------------------

.. proof:exercise::

  Construisez une autre classe  pour la méthode de Gauss-Seidel. **Validez** toujours vos codes avant de poursuire.


.. proof:remark::

  Contrairement à la méthode de Jacobi, la méthode de Gauss-Seidel requière la résolution d'un système linéaire triangulaire inférieur :

  - Surtout **n'extrayez pas** la partie triangulaire inférieure dans une nouvelle `Matrice` ! Pire encore, **ne l'inversez en aucun cas** !
  - Soyons astucieux et utilisons la fonction **résolvant un système linéaire triangulaire inférieur déjà implémentée** !


Classe Relaxation
-----------------

.. proof:exercise::
  
    Construisez une dernière classe pour la méthode de Relaxation. **Validez** toujours vos codes avant de poursuire.

.. proof:tips::

  Deux remarques d'importance :

  1. La méthode nécessite un paramètre :math:`\omega` : vous pouvez bien entendu ajouter ce paramètre à votre classe :code:`Relaxation`
  2. Cette méthode nécessite la résolution d'un système linéaire triangulaire inférieure. La même remarque que pour Gauss-Seidel s'applique ici : surtout pas d'extraction de matrice ! Vous pouvez créer une nouvelle fonction qui **résout un système linéaire triangulaire inférieur** et dont la diagonale est **multipliée par un paramètre** (ici : :math:`1/\omega`) !
