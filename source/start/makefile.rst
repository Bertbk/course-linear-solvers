Workflow : structure et Makefile
================================

C'est ici que commence vraiment les tps et le projet d'implémentation d'une bibliothèque. Mais avant de se lancer dans la programmation proprement dite, prenez 2 minutes pour préparer votre *workflow*.

Arborescence & Makefile
-----------------------

- Créez un nouveau dossier, par exemple :code:`tp_4m053`, dans lequel tout (absolument tout) ce qui suivra sera contenu dans ce dossier. 
- Notre projet de bibliothèque contiendra de nombreux fichiers, il est donc important de disposer d'un bon système de rangement. Pour cela, nous proposons de :

  - Placer les fichiers header (:code:`.hpp`) dans un dossier :code:`include` et les fichiers source (:code:`.cpp`) dans un dossier :code:`src`
  - Chaque fichier qui contient la fonction `main()` sera compilé pour fournir un binaire et sera stocké à la racine du dossier.
  - Chaque classe disposera d'un fichier header et d'un fichier source. Par exemple pour la classe :code:`Vecteur` :  `src/vecteur.cpp:code:` et `include/vecteur.hpp`. Ne mélangez pas 2 classes dans un même fichier !

- Compiler toutes la bibliothèque en utilisant directemenet :code:`g++` *à la main* serait laborieux. **À partir de maintenant**, nous utiliserons `un fichier Makefile <https://en.wikipedia.org/wiki/Makefile>`_. C'est un type de fichier utilisé par le programme :code:`make` qui permet d'exécuter un ensemble d'actions. Il permet notamment d'automatiser la compilation des fichiers séparés, de telle sorte qu'une commande unique suffise pour ne (re)compiler que les fichiers nécessaires.  Nous vous en fournissons un que vous pouvez télécharger avec le lien ci-dessous et ensuite le placer à la racine de votre dossier :


`Téléchager le Makefile <https://plmlab.math.cnrs.fr/4m053/example/raw/master/Makefile>`_


Au final, votre arborescence ressemble à cela :

.. code-block:: bash

  tp_4m053/
      └── Makefile     # Fichier makefile
      └── main.cpp     # Fichiers contenant "int main()"
      └── test.cpp     # Fichiers contenant "int main()"
      └── ...             
      └── include/     # Contient les fichiers header
            └── vecteur.hpp
      └── src/         # Contient les fichiers sources
            └── vecteur.cpp

Et la compilation de vos programmes s'effectue avec un simple :code:`make` dans le terminal au même niveau que les autres fichiers :

.. code-block:: bash

  make # compilation
  bin/main # exécution (si le fichier compilé s'appelle main évidemment)

.. proof:tips::

  Pour compiler un nouveau fichier "main", il faut modifier la ligne :code:`EXEC = main` et ajouter, à la suite et espacé d'un "espace", le nom des fichiers (*e.g.* :code:`EXEC = main test`). D'autres commandes sont de plus disponibles :

  - :code:`make clean` : pour supprimer les fichiers objets et l'exécutable. Les binaires construits sont stockés dans le dossier :code:`bin` (comme *binaries*) tandis que les objets (:code:`.o`) le sont dans le dossier :code:`obj` (*mais ceux là on ne les regarde jamais*)
  - :code:`make debug` : pour [compiler en mode debug]({{<relref "../help/debug.md">}}), les binaires ainsi compilés sont placés dans le dossier :code:`debug`.

.. proof:tips::

  Si vous utilisez git, vous pouvez `télécharger le dépôt exemple tout prêt <https://plmlab.math.cnrs.fr/4m053/example>`_ proposé par nos soins (vous devrez supprimer les fichiers :code:`main.cpp`, `src/hello.cpp:code:` et `include/hello.hpp`) :

  .. code-block:: bash

    git clone https://plmlab.math.cnrs.fr/4m053/example.git tp_4m053
    cd 4m053
    git remote remove origin


Conseils
--------

Éditeur de textes
+++++++++++++++++

Sur les machines de l'université, `Emacs <https://www.gnu.org/software/emacs/>`_ est accessible et sera votre meilleur allié (par rapport à :code:`gedit` par exemple ...) :

- Découpage de la fenêtre
- Indentation automatique
- ...

Sur vos machines personnelles, `Emacs <https://www.gnu.org/software/emacs/>`_ est bien entendu installable. Si vous préférez un logiciel plus *user-friendly*, nous vous conseillons l'excellent `VS Code <https://code.visualstudio.com/>`_ avec ses extensions, notamment pour le C++.


Multipliez les exécutables
++++++++++++++++++++++++++

Crééez autant de fichiers "main" que vous voulez ! Évitez surtout d'avoir un programme "main" gigantesque avec de grosses portions commentées selon les cas tests ! Préférez plusieurs petits programmes réalisant de petites opérations.

.. proof:remark::

  Un exemple simple : après avoir implémenté les matrices et vecteurs, vous disposez d'un petit programme qui construit deux matrices, deux vecteurs, les affichent et vérifie aussi que les différentes opérations :code:`+`, :code:`-`, :code:`*` sont valides. Le programme est testé et validé : parfait ! On le garde pour plus tard, on n'y touche plus. 

  Ce programme nous servira de référence. Si, plus tard, il ne fonctionne plus, on en déduira que quelque chose a été cassé entre temps...

Utilisez git
++++++++++++

Et faites de nombreux commits (et *pushez* sur votre serveur !). N'hésitez pas à vous référez :ref:`à la section d'aide <sec-git>`

Makefile
++++++++

Voici quelques liens pour vous familiariser avec les :code:`Makefile` :

- `Developpez.com <http://gl.developpez.com/tutoriel/outil/makefile/>`_
- `Cours sur Makefile <http://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/>`_
- `Manuel du logiciel make <https://www.gnu.org/software/make/manual/>`_