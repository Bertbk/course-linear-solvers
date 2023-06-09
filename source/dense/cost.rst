.. _sec-cpu-cost:

Coût Mémoire
============

Problématique du Stockage
-------------------------

La taille mémoire occupée par un :code:`double` en mémoire dépend de la machine, mais occupe en général 8 octets et c'est ce que nous supposerons ici. Une matrice de taille N×N stockée de manière dense occupe alors 8×N² octets en mémoire. Le tableau ci-dessous présente le coût mémoire dû uniquement au stockage de la matrice `en mébioctet (Mio) ou gibioctet (GiO) <https://fr.wikipedia.org/wiki/Octet>`_ :

+---------+---------+---------+---------+---------+---------+---------+---------+---------+
| N       | 100     | 500     | 1000    | 5000    | 10000   | 20000   | 50000   | 100 000 |
+=========+=========+=========+=========+=========+=========+=========+=========+=========+
| Mémoire | 0.08Mio | 1.9Mio  | 7.6Mio  | 190Mio  | 763Mio  | 2.9Gio  | 18Gio   | 74Gio   |
+---------+---------+---------+---------+---------+---------+---------+---------+---------+

Autrement dit, une matrice de taille 20000×20000 prend déjà presque 3Gio de mémoire ! Avec les ordinateurs portables modernes, cela reste envisageable mais ne l'est plus dès que N atteint 50000 ! 

Conclusion
----------

La problématique ne se pose pas vraiment pour des matrices de petite taille (N ~ 1000). Gardons à l'esprit que notre but est de résoudre des problèmes de grande taille, avec N de l'ordre de 10000 et nous devons **évitez de duppliquer inutilement** des :code:`Matrice` :

1. **Passage d'argument par référence ou pointeur** et non par recopie
2. Même dans le cas où il est facile à calculer, **l'inverse d'une matrice ne sera jamais explicitement calculé/stocké** 


