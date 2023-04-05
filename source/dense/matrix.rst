Matrices
========

Objectifs
---------

1. Implémenter une classe :code:`Matrice` pour manipuler des matrices carrées
2. Implémenter des opérations entre les classe :code:`Matrice` et :code:`Vecteur`

Fichiers
--------

Tout comme les vecteurs, il est largement préférable de créer un fichier header et un fichier source pour les matrices.

Stockage
--------

La classe :code:`Matrice` représente des matrices **carrées** (uniquement) et les stocke sous forme **dense**. Cette classe permettra d'effectuer des opérations matricielles (multiplication, ...). Elle contient comme données privées (au moins, libre à vous d'en ajouter):
 
- :code:`int N_` : Le nombre de colonnes (= de lignes)
- :code:`std::vector<double> coef_` : Les coefficients, stockés dans un tableau à une dimension de la bibliothèque standard

Les coefficients sont stockés de la façon suivante. Le coefficient positionné en $(i,j)$ dans la matrice sera à la position :code:`j+i*N_` dans le vecteur de coefficients, où :code:`N_` est le nombre de colonnes ou de lignes.

Comme pour la classe :code:`Vecteur`, la classe :code:`Matrice` comporte des constructeurs et son fichier header ressemble ainsi à ceci

.. code-block:: cpp

  // Fichier include/matrice.hpp
  #pragma once
  #include<vector>   // pour les std::vector
  #include<iostream> // pour std::ostream

  class Matrice{
  private:
    int N_;
    std::vector<double> coef_;
  public:
    Matrice (); // constructeur vide
    Matrice (int N); // constructeur créant une matrice nulle de taille N
    Matrice (const Matrice & M); // constructeur par recopie
    ~Matrice() = default;
  };

  std::ostream & operator<<(std::ostream & os, const Matrice &A);


Quelques méthodes
-----------------

1. Méthode (constante) qui renvoie la taille de la matrice
2. Les 2 accesseurs (l'un pour modifier, l'autre pour obtenir la valeur) :

  .. code-block:: cpp

    double & operator() (int i, int j);     // Accès à la référence
    double operator() (int i, int j) const; // Accès à la valeur (recopie)

3. Une Fonction qui affiche la matrice dans le terminal en utilisant les flux (en utilisant l'opérateur :code:`(i,j)` et non directement le vecteur :code:`coef_` !)

  .. code-block:: cpp

    std::ostream & operator<<(std::ostream & os, const Matrice &A);


.. proof:warning::

  À partir de maintenant, vous ne devriez plus jamais accéder aux coefficients via :code:`coef_` mais **uniquement via l'opérateur parenthèse** :code:`()` !

  En effet, si nous décidons de modifier la façon dont sont stockés les coefficients, nous aurons à répercuter cette modification  **uniquement** dans les :code:`operator()`.


Opérations élémentaires
-----------------------

En mathématique, une matrice n'est pas qu'un tableau de coefficients et des opérations comme la multiplication ou l'addition sont possibles : ne nous privons pas de les implémenter ! Comme pour la classe :code:`Vecteur`, nous vous invitons à définir ses fonctions **en dehors de la classe** :code:`Matrice` et **sans utiliser de** :code:`friend` :

1. L'addition et la soustraction entre deux matrices en surchargeant les opérateur :code:`+` et :code:`-`
2. La multiplication par une autre :code:`Matrice`
3. La multiplication par un scalaire

.. proof:exercise::

  En vous inspirant de ce qui a été réalisé pour la classe :code:`Vecteur`, améliorez la classe :code:`Matrice` avec les opérations arithmétiques précédentes (et d'autres si vous le souhaitez).

Produit Matrice-Vecteur
-----------------------

Implémentez le produit matrice vecteur sous forme d'un :code:`operator*` :

.. code-block:: cpp
  
  Vecteur operator*(const Matrice& A, const Vecteur& x);

.. proof:remark::

  N'oubliez pas, alors, d'inclure le fichier header des vecteurs.


.. proof:remark::

  Quelques astuces générales :

  - Utilisez **des références en argument** pour éviter les **recopies inutiles** d'objets qui peuvent être lourdes en mémoire, ce qui est le cas pour les matrices.
  - Dans le cas des références passées en argument, **déclarez les constantes** (:code:`const truc &`) si l'argument n'a pas vocation à être modifié par la fonction.
  - De même, déclarez **les méthodes de vos classes comme constantes** si l'objet appelant n'est pas modifié.


Résumé en diagrame
------------------


.. raw:: html

  <div class="mermaid">
  classDiagram
    class Matrice{
      -int N_
      -std::vector double coef_
      +Matrice()
      +Matrice(int N)
      +Matrice(const Matrice & )
      +~Matrice()
      +int size() const
      +double operator#40;#41;(int i, int j) const
      + & double operator#40;#41;(int i, int j)
      }
  </div>