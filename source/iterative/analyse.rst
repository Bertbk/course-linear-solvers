Analyse Numérique
=================

Objectifs
---------

Pour les différentes méthodes itératives standards, nous souhaitons comparer :

1. **Estimer la vitesse de convergence** (*ie* le nombre d'itérations) 
2. **Comparer cette estimation** à celle **obtenue numériquement**


Problème modèle
---------------

Nous disposons maintenant d'une implémentation des trois principales méthodes itératives standards : nous devons maintenant les analyser et les comparer. Pour cela, nous utilisons la matrice :math:`A_N` de taille :math:`N\times N`:

.. math::

  A_N =
  \begin{pmatrix}
    2 & -1 & 0 & 0 & \ldots & 0 & 0\\
    -1 & 2 & -1 &  0 & \ldots & 0 & 0\\
      0 & -1 & 2 & -1 & \ldots & 0 & 0 \\
      \vdots & \ddots& \ddots& \ddots & \ldots & \vdots  & \vdots\\
      0 & 0 & 0 & 0 & \ldots & -1 & 2 \\
  \end{pmatrix}


Les :math:`N` valeurs propres de la matrice :math:`A_N` sont données par, pour :math:`k=1,\ldots, N` :

.. math::
  
  \lambda_k = 4 \sin^2\left(\frac{k\pi}{2(N+1)}\right).

En notant :math:`L :=M^{-1}(M - A) = I - M^{-1}A` la matrice d'itération de la méthode itérative (Jacobi, Gauss-Seidel, Relaxation) et :math:`\rho(L)` son `rayon spectral <https://en.wikipedia.org/wiki/Spectral_radius>`_, alors pour atteindre une erreur relative de :math:`\varepsilon`, le nombre d'itérations estimé est donné par la formule :

.. math::
  :label: eq-niter
  
  n_{iter} = \frac{{ \ln}(\varepsilon)}{{ \ln}(\rho(L))},


En particulier, si nous connaissons le rayon spectral de la matrice d'itération, alors nous disposons d'une estimation du nomre d'itérations nécessaires pour aboutir à la convergence.

.. proof:tips::
  
  Notez que cette matrice :math:`A_N` fait partie :ref:`des matrices de test régulière <sec-test-matrices>`. 


Méthode de Jacobi
-----------------

Nombre d'itérations : analytique
++++++++++++++++++++++++++++++++

Nous cherchons à estimer le nombre d'itérations de la méthode pour une tolérance :math:`\varepsilon` donnée, tout d'abord pour la méthode de Jacobi. Pour cette technique, nous rappelons que :math:`M = {\rm diag}(A)` ce qui, dans notre cas, se simplifie par :math:`M = \frac{1}{2}I` où :math:`I` est la matrice identité. La matrice d'itération :math:`J` est alors donnée par :

.. math::
  
  J = I - ({\rm diag}(A))^{-1}A = I - \frac{1}{2}A.
 
Les valeurs propres :math:`\mu_k(J)` de :math:`J` vérifient donc :math:`\mu_k(J) = 1 -\frac{\lambda_k}{2}`. En utilisant la propriété 

.. math::
  
  \sin^2\left(\frac{k\pi}{N+1}\right) = \frac{1-\cos\left({\frac{k\pi}{N+1}}\right)}{2},

nous obtenons l'expression des valeurs propres de :math:`J` :

.. math::
  
  \mu_k(J) = \cos\left(\frac{k\pi}{N+1}\right).

Ensuite, en utilisant le fait que :math:`\cos(\pi - x) = \cos(x)`, nous en déduisons le rayon spectral de :math:`J` :

.. math::
  :label: eq-rhoj

  \rho(J) = \cos\left(\frac{\pi}{N+1}\right).

Nous en déduisons le nombre d'itérations :

.. math::
  :label: eq-iterJanalytique

  n_{iter}(J) = \frac{\ln(\varepsilon)}{\ln\left(\cos\left(\frac{\pi}{N+1}\right)\right)}.


Nombre d'itérations : estimation par DL
+++++++++++++++++++++++++++++++++++++++

L'expression :eq:`eq-iterJanalytique` est très précise mais difficilement interprétable pour les humains que nous sommes. Nous calculons ici une estimation de cette estimation. Pour cela, après avoir appliqué `un développement limité de Taylor <http://www.h-k.fr/publications/data/adc.ps__annexes.maths.pdf>`_ à l'ordre 2 de :math:`\rho(J)` lorsque :math:`N\to+\infty`, nous obtenons :

.. math::
  
  \rho(J) = 1 - \frac{\pi^2}{2(N+1)^2} + O\left(\left(\frac{1}{N+1}\right)^4\right).

En reportant cette relation dans l'équation :eq:`eq-niter`, nous en déduisons une estimation du nombre d'itérations :

.. math::
  :label: eq-iterJ

  n_{iter}(J) \simeq - 2\frac{\ln(\varepsilon)}{\pi^2}(N+1)^2.


.. proof:exercise::

  Optionnel : refaites les calculs.

Comparaison des estimations avec le numérique
+++++++++++++++++++++++++++++++++++++++++++++

Nous fixons la tolérance :math:`\varepsilon = 10^{-1}` et le nombre maximal d'itérations :math:`n_{max} = 10^5`. Nous pouvons calculer deux estimations du nombre d'itérations nécessaires : 

1. "Analytique" : l'équation :eq:`eq-iterJanalytique`
2. "DL" : l'équation :eq:`eq-iterJ`

Ensuite, nous pouvons bien entendu calculer le nombre d'itérations de manière numérique (*ie* : en pratique par l'ordinateur) et comparer avec les estimations.

.. proof:exercise::

  Pour N=10, 50 et N=100, calculez ces trois valeurs : estimation "analytique", estimation "Dév. Lim." et le nombre d'itérations obtenu numériquement :

  +----------------------+--------------------------------------+-----------------------+------------+
  | Nb. d'iterations...  | Analytique :eq:`eq-iterJanalytique`  | "DL" :eq:`eq-iterJ`   | Numérique  |
  +======================+======================================+=======================+============+
  | :math:`N = 10`       | ?                                    |                     ? | ?          |
  +----------------------+--------------------------------------+-----------------------+------------+
  | :math:`N = 50`       | ?                                    |             ?         | ?          |
  +----------------------+--------------------------------------+-----------------------+------------+
  | :math:`N = 100`      | ?                                    |            ?          | ?          |
  +----------------------+--------------------------------------+-----------------------+------------+

  Que pouvez-vous en conclure sur les estimations du nombre d'itérations ? Est-ce que :eq:`eq-iterJ` est satisfaisante et si oui, à partir de quelle valeur de N ? 


Méthode de Gauss-Seidel
+++++++++++++++++++++++

Comme la matrice :math:`A_N` est tri-diagonale, le rayon spectral de la matrice de Gauss-Seidel :math:`\rho(G)` est donnée par :math:`\rho(G) = \rho(J)^2`. Nous pouvons ainsi en déduire :

.. math::

  \begin{array}{r c l}
  \rho(G) &=&\displaystyle \rho(J)^2 \\
  &=&\displaystyle \cos\left(\frac{\pi}{N+1}\right)^2\\
  &=&\displaystyle  \left(1 - \frac{\pi^2}{2(N+1)^2} + O\left(\left(\frac{1}{N+1}\right)^4\right)\right)^2\\
  &=&\displaystyle  1 - \frac{\pi^2}{(N+1)^2} + O\left(\left(\frac{1}{N+1}\right)^4\right)
  \end{array}

Nous pouvons alors en déduire une estimation du nombre d'itérations :

.. math::
  :label: eq-iterG

  n_{iter}(G) \simeq - \frac{\ln(\varepsilon)}{\pi^2}(N+1)^2 \simeq \frac{n_{iter}(J)}{2}.


.. proof:exercise::

  Comparez le nombre d'itérations théoriques, estimés et pratiques pour N=10, 50 et N=100.

Méthode de Relaxation
+++++++++++++++++++++

Pour la méthode de relaxation, comme :math:`A_N` est triagonale alors le paramètre optimal :math:`\omega^*` pour la méthode de relaxation est donné par

.. math::

  \omega^* = \frac{2}{1 + \sqrt{1 - \rho(J)^2}},

et le rayon spectral de la matrice d'itération est alors donné par :math:`\rho(G_{\omega^*}) = \omega^* - 1`. Ci-dessous une courbe du rayon spectrale en fonction de :math:`\omega` pour N=10 :

.. raw:: html

  <div id="relaxation"></div>


.. proof:remark::

  Pour :math:`\omega \geq \omega^*`, nous avons :math:`\rho(G_{\omega}) = \omega - 1`. La courbe ci-dessus montre qu'il est préférable de choisir :math:`\omega` légèrement plus grand que :math:`\omega^*` plutôt que plus petit.


Quand :math:`N\to+\infty`, nous obtenons le développement limité de :math:`\omega^*` :

.. math::

  \omega^* = 2\left(1 - \frac{\pi}{N+1}  \right)+O\left(\left(\frac{1}{N+1}\right)^2\right).

Nous pouvons en déduire une estimation du nombre d'itérations


.. math::
  :label: eq-iterR

  n_{iter}(G_{\omega^*}) \simeq - \frac{\ln(\varepsilon)}{\pi^2}(N+1).

La dépendance en :math:`N` est maintenant linéaire et non plus quadratique !

.. proof:exercise::

  Comparez le nombre d'itérations théoriques, estimés et pratiques pour N=10, 50 et N=100.

.. raw:: html

  <script defer type="text/javascript" src="../../_static/js/relaxation.js"></script>
