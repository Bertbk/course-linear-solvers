.. _sec-matplotlib:

Matplotlib : Visualisation des Données
======================================

Objectifs
---------

1. Apprendre les fonctionnalités de base de `Matplotlib <https://matplotlib.org/>`_
2. Afficher les courbes associées aux données stockées :ref:`au format JSON <sec-json>`

Documentation
-------------

- `Tutoriels Pyplot <https://matplotlib.org/tutorials/introductory/pyplot.html#sphx-glr-tutorials-introductory-pyplot-py>`_
- `Usage Guide <https://matplotlib.org/tutorials/introductory/usage.html#sphx-glr-tutorials-introductory-usage-py>`_

Contexte
--------

Nous supposons que nous disposons du fichier JSON suivant, nommé :code:`data_iterative.json` et qui nous servira de cas test pour nos scripts Python :

.. code-block:: json

  {
    "jacobi" : {
      "method" : "Jacobi",
      "N" : 5,
      "n_iter" : 17,
      "resvec" : [0.836660, 0.724569, 0.627495, 0.543427, 0.470621, 0.407570, 0.352966, 0.305677, 0.264724, 0.229258, 0.198543, 0.171944, 0.148908, 0.128958, 0.111681, 0.096718],
      "cpu_time" : 0.000277
    },
    "gauss" : {
      "method" : "Gauss Seidel",
      "N" : 5,
      "n_iter" : 10,
      "resvec" : [0.793159, 0.625781, 0.493288, 0.383885, 0.291933, 0.219996, 0.165263, 0.124014, 0.093027],
      "cpu_time" : 0.000126
    },
    "relaxation" : {
      "method" : "Relaxation",
      "N" : 5,
      "omega" : 0.952802,
      "n_iter" : 11,
      "resvec" : [0.798033, 0.641270, 0.515370, 0.410401, 0.321706, 0.250058, 0.193648, 0.149731, 0.115701, 0.089382],
      "cpu_time" : 0.000145
    }
  }

Affichage de courbes
--------------------

Nous proposons d'utiliser la bibliothèque :code:`json` pour *parser* (=lire) le fichier JSON. Le petit script ci-dessous affiche les courbes de convergence associées à la quantité :code:`resvec` des clés :code:`jacobi` et :code:`gauss`, du fichier JSON précédént :

.. code-block:: python

  # Python 3
  import json
  import matplotlib.pyplot as plt

  # Importation des données depuis le fichier JSON
  # ===============================================
  # Ouverture du fichier
  data_file = open('data_iterative.json', 'r')
  # Parse et stockage dans data
  data = json.load(data_file)
  # Fermeture du fichier
  data_file.close()

  # Matplotlib
  # ==========
  # Plot de chaque courbe
  plt.plot(data['jacobi']['resvec'], label=data['jacobi']['method'])
  plt.plot(data['gauss']['resvec'], label=data['gauss']['method'])
  plt.plot(data['relaxation']['resvec'], label=data['relaxation']['method'])
  # Légende, axes et titres
  plt.legend(loc='best')
  plt.xlabel('Iteration Number')
  plt.ylabel('$\log_{10}(\|r\|/\|b\|)$')
  plt.title('Convergence histories for N=' + str(data['jacobi']['N']))
  # Log scale pour y
  plt.yscale("log")
  # Affichage
  plt.show()

Vous obtenez la figure suivante :

.. figure:: /img/matplotlib_fig1.*
  :figwidth: 100%
  :width: 100%
  :alt: Courbes de convergence
  :align: center

  Courbes de convergence


Automatisation : :code:`for`
----------------------------

Les trois lignes correspondant à l'affichage :

.. code-block:: python

  # Plot de chaque courbe
  plt.plot(data['jacobi']['resvec'], label=data['jacobi']['method'])
  plt.plot(data['gauss']['resvec'], label=data['gauss']['method'])
  plt.plot(data['relaxation']['resvec'], label=data['relaxation']['method'])

peuvent être automatisées dans une boucle :code:`for` :

.. code-block:: python
  
  # Plot de chaque courbe
  for key, d in data.items():
    plt.plot(d['resvec'], label=d['method'])

Multiples Figures
-----------------

Matplotlib est très souple et permet par exemple d'afficher des sous-figures avec `la commande subplot <https://matplotlib.org/api/_as_gen/matplotlib.pyplot.subplot.html#matplotlib.pyplot.subplot>`_. Reprenant le code précédent, nous pouvons afficher les trois courbes séparément :

.. code-block:: python

  import json
  import matplotlib.pyplot as plt

  data_file = open('data_iterative.json', 'r')
  data = json.load(data_file)
  data_file.close()

  # Matplotlib
  plt.subplot(221) # 221 = nb de lignes : 2, nb de colonnes : 2, pointeur sur la case : 1
  plt.plot(data['jacobi']['resvec'], 'r--')
  plt.yscale("log")
  plt.grid(True)
  plt.xlabel('Iteration Number')
  plt.ylabel('$\log_{10}(\|r\|/\|b\|)$')
  plt.title(data['jacobi']['method'])
  # pointeur sur la case 2
  plt.subplot(222)
  plt.plot(data['gauss']['resvec'], 'k-.')
  plt.yscale("log")
  plt.grid(True)
  plt.xlabel('Iteration Number')
  plt.ylabel('$\log_{10}(\|r\|/\|b\|)$')
  plt.title(data['gauss']['method'], 'r')
  # pointeur sur la case 3
  plt.subplot(223)
  plt.plot(data['relaxation']['resvec'], 'b.-')
  plt.yscale("log")
  plt.grid(True)
  plt.xlabel('Iteration Number')
  plt.ylabel('$\log_{10}(\|r\|/\|b\|)$')
  plt.title(data['relaxation']['method'])
  # Titre supérieur global
  plt.suptitle('Convergence histories for N=' + str(data['jacobi']['N']))
  # Affichage
  plt.tight_layout() # évite les chevauchements entre figures 
  plt.subplots_adjust(top=0.85) # ajuste la box
  plt.show()


.. figure:: /img/matplotlib_fig_multiple.*
  :figwidth: 100%
  :width: 100%
  :alt: Exemple de sous-figures
  :align: center

  Exemple de sous-figures


Options
-------

Les deux scripts fournis ne font que survoler les capacités de Matplotlib qui dispose `d'une quantité impressionante d'options <https://matplotlib.org/api/_as_gen/matplotlib.lines.Line2D.html#matplotlib.lines.Line2D>`_, allant de la couleur et de l'épaisseur de la courbe au type de courbe (barres, scatters, ...) :

.. code-block:: python

  plt.plot(..., 'r') # en rouge et ligne solide
  plt.plot(..., 'k--') # en noir pointillé
  plt.plot(..., 'b-.') # en bleu ligne pointillé / solide
  plt.plot(..., 'b.')  # en bleu avec juste les points (pas de ligne)
  plt.plot(..., 'b.-') # en bleu les points et les lignes
  etc.