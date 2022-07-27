.. Linear Solvers documentation master file, created by
   sphinx-quickstart on Sun Jul 17 19:08:58 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

************
Introduction
************

Deux objectifs couplés
======================

Les TPs associés au cours 4M053 "Calcul scientifique pour les grands systèmes linéaires" ont deux objectifs :

1. **Implémenter** une bibliothèque en C++ pour :

   * Gérer des matrices sous différents formats (stockage, opérations usuelles, ...)
   * Résoudre des systèmes linéaires avec les solveurs étudiés en cours (directs, itératifs, ...)

2. **Analyser** le comportement des solveurs et des méthodes de stockage à l'aide de cette bibliothèque

Langages choisis
================

Pour le développement : C++
-----------------------------------

Ces TPs vous demandent d'être **familié avec au moins un langage compilé** (:code:`C`, C++, :code:`Fortran`, ...) et le langage retenu ici sera le C++. Si vous n'êtes ni familié avec un langage compilé ni avec le C++, vous allez devoir **travailler dure** pour rattraper le retard : ces TPs **ne sont pas** des TPs d'informatique ! Rien n'est impossible mais nous **ne ferons pas le travail à votre place**. Nous avons mis en place  une section contenant des liens pour vous aider (TODO: link).

Pour l'analyse : :code:`Python`, :code:`Julia`, :code:`MATLAB`, ...
-------------------------------------------------------------------

À vous de décider le langage que vous préférez pour afficher les courbes et traiter les données. Nous vous incitons à utiliser `Python <https://www.python.org/>`_ avec les bibliothèques `Matplotlib <https://matplotlib.org/>`_ pour l'affichage et `Numpy <https://www.numpy.org/>`_ pour toute opération de calcul scientifique (matrice, vecteur, ...). À noter que `Julia <https://julialang.org>`_ peut également appeler Matplotlib.

Dans la rubrique d'aide, nous fournissons un script Python (TODO:link json) permettant de lire un fichier :code:`JSON`, d'en extraire des données et d'afficher une courbe.

Organisation
============

La partie "Premiers pas" (TODO: link start/hello-world.md) présente la compilation pour le C++ et surtout introduit une organisation possible pour le code et les fichiers. Les bonnes pratiques de programmation, à prendre *dès le début*, seront aussi soulignées. Ensuite seulement commencera l'implémentation de la bibliothèque.

Environnement
=============

OS
---

Les terminaux mis à disposition tournent sous Linux et disposent des outils nécessaires (voir ci-après). Vous pouvez cependant utiliser votre propre machine, mais **nous n'assurons pas le SAV** dans ce cas et nous vous invitons à utiliser Linux ou Mac OS plutôt que Windows.

.. _sec-software:

Logiciels
---------

Sauf si vous disposez déjà de votre propre environnement de travail (*workflow*), alors c'est parfait ! Autrement, nous vous suggérons les outils suivants. 

Traitement de textes
--------------------


Un éditeur de textes peut intégrer un certain nombre d'outils qui peuvent vous aider tels qu'un indenteur automatique, un débogueur, un compilateur, un analyseur statique, un profiler, etc. Maîtriser un (bon) éditeur de texte est un investissement (très) rentable ! De base, vous disposez de GEdit, mais ce traitement de texte est (trop) basique. Voici trois exemples :

- `VSCode <https://code.visualstudio.com/>`_ : intuitif avec une prise en main rapide et `rempli de packages très agréables <https://ljll.math.upmc.fr/infomath/tools/vscode>`_. VSCode est disponible sous windows, mac et linux. Probablement et malheureusement pas disponible avec les terminaux de l'université.
- `Emacs <https://www.gnu.org/software/emacs/>`_ : extrêmement puissant mais avec une courbe d'apprentissage assez raide. Très probablement disponible. 
- `Vim <https://www.vim.org/>`_ : tout aussi puissant qu'emacs et avec une courbe d'apprentissage tout aussi raide.

Gérez vos sources avec `Git <https://git-scm.com/>`_
------------------------------------------------------------

Stockez votre dépôt sur `Github <https://github.com>`_ (En tant qu'étudiant(e) vous pouvez souscrire au `Student Developper Pack <https://education.github.com/pack>`_) ou `Gitlab <https://gitlab.com>`_. Notez que des packages pour utiliser Git directement dans le traitement de textes sont disponibles pour VScode, Emacs et Vim. Une section d'aide y est consacrée (TODO: link git).

.. proof:remark::

  Vous ne savez pas utiliser Git ? C'est le moment d'apprendre ! Nous n'avons toutefois et malheureusement pas le temps d'apprendre à utiliser cet outil - pourtant extrêmement utile ! Nous vous **encourageons fortement** à apprendre à vous en servir.


Contribuez !
============

Vous avez repéré une erreur : grammaire, typo, de mathématiques ou d'informatique ? Ou avez des suggestions ? C'est le moment de participer ! Le code des tps est en `accès libre sur github <https://github.com/Bertbk/course_4m053>`_ (le lien est aussi fourni sur le menu à droite). Vous pouvez alors soumettre `un Pull Request <https://help.github.com/articles/about-pull-requests/>`_ :

1. `Forker le dépôt <https://help.github.com/articles/fork-a-repo/>`_ dans votre compte github
2. Récupérer le code sur votre ordinateur avec la commande :code:`git clone`
3. Créer une branche dédié

   .. code-block:: bash

      git branch correction
      git checkout correction

4. Effectuer les modifications
5. Versionner et envoyer au serveur

   .. code-block:: bash

      git commit -m "blabla" -a
      git push

6. Retourner sur `le dépôt du cours <https://github.com/Bertbk/course_4m053>`_ : on vous propose de faire un Pull Request
7. De notre côté, nous validerons (ou pas ;-))




Comment lire ces TPs
====================

Nous vous conseillons de suivre les étapes **dans l'ordre**. Tout au long des TPs, des exercices vous seront proposés. 


.. toctree::
   :caption: Table des matières :
   :hidden:
   
   help/index
   start/index
   dense/index
   direct/index
   