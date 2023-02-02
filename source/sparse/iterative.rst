Gradient Conjugué creux
=======================

Objectifs
---------

- Implémenter le Gradient Conjugué prenant en charge une matrice CSR
- Comparer les temps CPU entre le Gradient Conjugué "Dense" et "CSR"


Gradient Conjugué "Creux"
-------------------------

Implémentation
^^^^^^^^^^^^^^

La méthode du Gradient Conjugué nécessite, à chaque itération, un produit matrice-vecteur et aucune inversion de matrice. Nous pouvons implémenter très facilement une version **creuse** du Gradient Conjugué. Sous réserve de disposer d'une classe :code:`MatriceCSR` fonctionnelle et utilisant le **même nom pour le produit matrice vecteur** que la classe :code:`Matrice`, alors la seule différence entre la version **dense** et **creuse** du Gradient Conjugué est le type du paramètre :code:`const Matrice & A_` qui devient :code:`const MatriceCSR &A_`.


.. proof:exercise::

  Implémenter une nouvelle classe pour résoudre le Gradient Conjugué dont la matrice est au format CSR.


.. proof:tips::

  `Le templating <https://cplusplus.com/doc/oldtutorial/templates/>`_ de votre classe du Gradient Conjugué est possible mais alors il faut fusionner le fichier :code:`.cpp` dans le :code:`.hpp`.



Performances
^^^^^^^^^^^^

.. proof:exercise::

  Comparez les performances, en terme de temps CPU, entre la méthode du Gradient Conjugué **dense** et **creux** pour une matrice de taille suffisamment importante.


.. proof:tips::

  Le nombre d'itérations entre la version Dense et Creuse doit être exactement le même !
