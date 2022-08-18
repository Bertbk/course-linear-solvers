.. _sec-cpu-time:

Temps CPU
=========

Une quantité d'intérêt pour comparer les performances des méthodes est le *temps CPU* en secondes. Nous suggérons d'utiliser la fonction :code:`clock()` de la bibliothèque standard :code:`ctime`. Cette fonction renvoit un temps CPU (en "clock" de type :code:`clock_t`), il suffit donc de l'appeler juste avant et juste après l'action à mesurer, et d'effectuer la différence des deux. Voici un exemple de code minimaliste permettant d'obtenir le temps CPU mis pour effectuer une action. La variable :code:`CLOCKS_PER_SEC` est déjà codée en dure et permet de calculer le temps en seconde.

.. code-block:: cpp

  # include <ctime> 
  int main (){
  clock_t start, end ;
  start = clock();
  /* Entrez ici les opérations à mesurer ... */
  end = clock();
  double temps_en_secondes = static_cast<double>(end - start) / CLOCKS_PER_SEC ;

.. proof:remark::

  Si votre temps est inférieur à la milliseconde, vous risquez de ne mesurer que du bruit. Vous pouvez, au choix :

  - [Moins bien] Effectuer N fois la même opération et diviser ensuite le temps par N
  - [Mieux] Augmenter la taille du problème

