C++
===========

Quelques habitudes à prendre
----------------------------

1. Le nom du fichier doit refléter sa **fonctionnalité**, choisissez des noms cohérents

  :code:`fonctions`, :code:`fichier`, :code:`tp1`, ... ne donnent aucune indication sur le contenu !

2. **Une fonction = Une fonctionnalité**. Autrement dit, une fonction ne doit faire **que** ce que l'on attend d'elle (`KISS : Keep It Simple, Stupid <https://fr.wikipedia.org/wiki/Principe_KISS>`_). 

  Par exemple, une fonction dont le but est de créer et renvoyer un tableau **ne doit pas** afficher le tableau et encore moins modifier une variable ! Elle doit juste créer et renvoyer un tableau. Sauf cas pathologique, quand vous commandez un café vous ne souhaitez pas que le serveur y ajoute une sardine dedans, même avec le sourire.

3. Les fonctions et variables doivent avoir des noms compréhensibles et surtout en rapport avec leurs fonctions !

  :code:`int truc = 2` n'est pas très parlant

4. À chaque nouvelle fonctionnalité, suivez la procédure suivante :
  
   1. **Compilation** : tant qu'elle plante, on corrige [#f1]_, sinon...
   2. ... **Validation** ! Tant que le résultat n'est pas bon, on corrige, sinon...
   3. ... **Passer à la suite**


Concernant les classes
----------------------

.. _sec-cpp-param-name:

Nom des paramètres
++++++++++++++++++

Une :code:`class` en C++ dispose (souvent) de paramètres *privés* (:code:`private`) ou *publiques* (:code:`public`). Le choix du nom de ces variables n'est pas sans incidence. Nous vous conseillons de terminer le nom de ces variables par un "underscore" :code:`_`. Cela permet d'éviter toute confusion avec d'autres variables, par exemple :

.. code-block:: cpp

  class MaClasse{
    private:
      int N_;
    public:
      MaClasse(int N){ N_ = N;}
  };

Sans le underscore , nous pourrions confondre l'arguement :code:`N` (extérieure) avec la variable privée de la classe.

.. proof:remark::

  Vous n'avez pas le droit de `commencer un nom de variable par un underscore <https://stackoverflow.com/questions/228783/what-are-the-rules-about-using-an-underscore-in-a-c-identifier>`_. Ceci est réservé au compilateur. Au cours de votre carrière, vous croiserez de nombreux programmes qui, malhereusement, ne suivent pas cette règle...


Méthode :code:`const`
+++++++++++++++++++++

Une *méthode* est une *fonction membre* d'une classe. Si le prototype d'une méthode se termine par :code:`const` cela signifie (au compilateur) que la méthode n'altère pas les paramètres de l'instance de la classe. Prenez l'habitude de placez des :code:`const` quand il faut **dès le début**, sinon vous allez au delà de grands changements...

.. code-block:: cpp

  class MaClasse{
    private:
      int N_;
    public:
      MaClasse(int N){N_ = N;}
      // Méthode "constante" :
      void Print() const{std::cout << "N = " << N_;} 
      // Méthode non "constante" modifiant le paramètre N_
      void Set(int N) {N_ = N;}
  };

Où trouver de l'aide et des informations pertinentes ?
------------------------------------------------------

Google/Bing/DuckDuckGo/... seront vos bras droits dans la recherche d'information sur le C++. Citons tout de même les sites suivants qui sont des valeurs sûres :

- Pour la syntaxe : `cppreference.com <https://fr.cppreference.com/>`_ ou `cplusplus.com <http://www.cplusplus.com/>`_
- Pour des questions pointues : `Stack Overflow <http://stackoverflow.com/>`_
- L'excellente `FAQ de isocpp <https://isocpp.org/faq>`_ présentant des exemples de cas pathologiques

Tutoriels en ligne
------------------

Ces derniers ne manquent pas, tels que :

- `LearnC++ <https://www.learncpp.com/>`_
- `OpenClassRoom <https://openclassrooms.com/fr/courses/1894236-programmez-avec-le-langage-c>`_


Les erreurs classiques
----------------------

Avant de nous appeler et au risque de *patienter* longuement avant notre venue : Google est votre ami (pas pour tout mais pour ça oui). Mais en général, l'erreur est classique...

Erreur de compilateur
+++++++++++++++++++++

Le compilateur renvoie un message d'erreur ? **Stop.** On arrête tout : on doit débugguer. On ne continue surtout pas son code au risque de rendre l'erreur encore plus incompréhensible.

1. Regardez le **premier** message d'erreur, celui le plus en haut (d'autres erreurs découlent probablement de l'erreur primaire)
2. Essayez de lire et de comprendre

Quelques erreurs classiques :

1. Oublie d'un **point virgule** à la fin d'une commande
2. Oublie de **typer** une variable ou une fonction
3. Oublie de **déclarer** la fonction ou d'inclure le fichier d'en-tête contenant la fonction (ou sa déclaration)
4. Erreur de **typographie** : par exemple :code:`sdt::vector` au lieu de :code:`std::vector`

Certains :code:`warning` méritent aussi de s'y attarder. Enfin terminons par ce théorème (admis sur le tas).

.. proof:theorem::

  Si vous pensez avoir raison mais que le compilateur prétend que vous avez tort, alors vous avez tort.


Erreur au lancement
+++++++++++++++++++

La compilation se passe bien, le programme se lance mais ne donne pas le résultat voulu et/ou plante. **Stop.** On arrête tout. On débugue.

L'utilisation d'un débugueur (un vrai) est conseillé, comme gdb (notez qu'il y en a un dans VSCode). Nous utiliserons cependant la *mauvaise habitude* de débuguer à coup de :code:`cout << "coucou"` pour vérifier si le programme plante avant ou après une commande. Le but est de localiser l'erreur, ensuite en général, ça saute aux yeux.

1. **Segmentation Fault** : la pire erreur à débuguer. C'est un problème de mémoire: votre programme cherche à lire ou à écrire sur un emplacement mémoire pour lequel il n'a pas le droit de le faire. Pour les débusquer, c'est délicat car le programme peut fonctionner une fois, et pas la fois suivante. Le débuguage à cout de :code:`cout` est parfois mis à mal. En général il faut :

   1. Surveillez l'utilisation que vous avez des pointeurs/références
   2. Surveillez les sorties de vos fonctions : retournez vous une variable qui est, en fait, détruite ?

2. Le **résultat est faux** : bon courage, il faut mettre les mains dans le cambouis ! Car non, **l'erreur ne vient pas** de l'ordinateur, mais bien de **votre code**.

.. proof:theorem::

  Si vous "n'avez rien changé au code je vous le promet" mais que le programme ne fonctionne plus ou ne se compile plus alors vous avez modifié le code (et mal).


.. _sec-cpp-badhabits:

Quelques mauvaises habitudes
----------------------------

- Évitez le plus possible :code:`using namespace std` : `voir ici pourquoi <https://stackoverflow.com/a/1453605/14065>`_ et surtout **à prohiber** `dans les fichiers header <https://stackoverflow.com/questions/5849457/using-namespace-in-c-headers>`_ !
- Passage d'argument par recopie : en C++, les arguments sont **par défaut passés par copie**. Rendre **constant un argument passé par copie est alors inutile**. Nous banirons donc ce genre de pratique :

  .. code-block:: cpp

    ... fonction(const Type T){...}


- Passage d'argument par référence : cette pratique permet d'envoyer non pas la valeur mais l'adresse de la variable à la fonction :

  .. code-block:: cpp

    ... fonction(const Type &T){...}

- Cette méthode est très intéressante quand :code:`Type` **n'est pas élémentaire** (:code:`double`, :code:`float`, :code:`int`, ...) et nécessite une taille mémoire importante (tableaux (:code:`std::vector`) ou matrice par exemple) :

  .. code-block:: cpp

    ... fonction(const std::vector<double> & v, ...){...}

- Pour des **entités élémentaires**, c'est à la fois `inutile et potentiellement dangereux <https://stackoverflow.com/questions/4705593/int-vs-const-int>`_. D'autre part, l'optimisation effectuée par le compilateur perd en efficacité avec de tels arguments. Ainsi, les prototypes de ce type seront *persona non grata* :

  .. code-block:: cpp

    ... fonction(const double & d, const int & a, ...){...}

Quelques liens utiles
---------------------

- :code:`friend operator<<` ou :code:`operator<<` ? `That's the question <https://stackoverflow.com/questions/236801/should-operator-be-implemented-as-a-friend-or-as-a-member-function>`_
- :code:`operator` : `méthode ou fonction ? <https://stackoverflow.com/questions/4622330/operator-overloading-member-function-vs-non-member-function>`_
- :code:`const int &` `en argument de fonction ? <https://stackoverflow.com/questions/4705593/int-vs-const-int>`_

.. rubric:: Footnotes
  
.. [#f1] Vous apprendrez vite à comprendre ce que vous dit le compilateur
