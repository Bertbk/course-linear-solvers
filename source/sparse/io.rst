Input / Output
==============


Objectif
--------

Construire une matrice creuse à partir d'un fichier


:code:`MatriceCOO`
------------------

Le :ref:`format de fichier proposé précédemment <dense-io>` est en réalité parfaitement adapté au format COO. Implémentez, pour la classe :code:`MatriceCOO` des méthodes permettant de :

- Lire des fichiers aux formats présentés plus haut et de modifier l'objet appelant en fonction
- Sauvegarder une :code:`MatriceCOO` sur disque au format proposé

Comme pour les matrices denses, vous pouvez aller plus loin en implémentant un constructeur qui prend en argument un nom de fichier et construit la :code:`MatriceCOO`. Il s'utiliserait alors de la façon suivante (exemple !) :

.. code-block:: cpp

  MatriceCOO A("laplacien_A.txt");
