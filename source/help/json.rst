.. _sec-json:

JSON : Format de sortie
=======================

Contexte
--------

Nous serons amenés à calculer des quantités d'intérêts de différents types comme :

- Le temps CPU d'une fonction en secondes (:code:`double`)
- Le nombre d'itérations pour une méthode itérative (:code:`int`)
- Le vecteur des résidus (tableau de :code:`double`)
- Le nom de la méthode itérative (:code:`string`)
- La taille du système linéaire (:code:`int`)
- Date et heure de la simulation (:code:`date` ou :code:`string`)
- etc.

Si nous ne nous organisons pas un minimum, nous serons noyés sous un nombre important de données sans même savoir quels paramètres ont été utilisés pour obtenir les données (*nous parlons ici de vécu !*). Le choix du format de fichier de sortie et de leur organisation est alors primordiale. 

Il existe de nombreuses façon de stocker des données résultant de simulations numériques, comme une base SQL (en local ou non) ou un format de fichiers simple, comme le `CSV <https://fr.wikipedia.org/wiki/Comma-separated_values>`_, `JSON <https://fr.wikipedia.org/wiki/JavaScript_Object_Notation>`_ ou encore `ENO <https://eno-lang.org/>`_. L'utilisation d'un fichier binaire permet de réduire le coût de stockage de celui-ci au prix (trop) fort de l'illisibilité humaine. Nous privilégiron donc le format texte (ASCII). Le CSV est de loin le plus format le simple mais est un peu moins flexible que le format JSON, qui permet de stocker plusieurs jeux de données dans un même fichier, c'est pourquoi nous proposons de l'utiliser. Cependant, libre à vous d'utiliser celui que vous voulez !

.. proof:remark::

  Une tendance naturelle est d'utiliser le nom du fichier de sortie pour les paramètres, cependant c'est une méthode :

  - Peu ou pas pratique pour des paramètres à virgule
  - Peu ou pas pratique pour plus de 2 paramètres : "output_a_10.232942_b_201_c_34342.txt"

  Nous déconseillons tout simplement cette pratique d'un autre âge.

Format JSON
-----------

Généralités
+++++++++++

Pour simplifier, `le format JSON <https://fr.wikipedia.org/wiki/JavaScript_Object_Notation>`_ est un format spécialisé dans le transfert de données. À chaque clé (*key*) est associée une valeur (*value*) ou un tableau de valeurs (*array of values*). Par exemple :code:`"cpu_time"` associé à :code:`3.343:code:` ou :code:`"method"` à :code:`jacobi`. Vous pouvez ajouter autant de clés que vous le voulez. 

Un fichier JSON est compartimentés entre des accolades :code:`{` et :code:`}`. Par exemple, imaginons que l'on ait lancé une résolution la méthode de Jacobi et le paramètre N=4. Après résolution, nous obtenons un vecteur :code:`resvec_` contienant trois valeures 12.1, 8.2, 5.4 et 3.2 (*prise ici au hasard*) et la résolution a nécessité un temps CPU de 3.2s. Notre fichier de sortie pourrait ressembler à celui-ci :

.. code-block:: json

  {
    "jacobi": {
      "method": "jacobi",
      "N": 4,
      "ntier": 3,
      "resvec":  [ 12.1, 8.2, 5.4, 3.2],
      "cpu_time": 3.2323,
    },
  }

Un tel format présente l'avantage d'être lisible par une personne mais aussi par Python ou JavaScript (d'où le nom de JSON) à l'aide de bibliothèques dédiées. En particulier, un script mis à disposition en fin de page permet de lire un tel fichier au format JSON et affiche la courbe associée à la clé :code:`resvec` en échelle logarithmique. Vous pouvez alors librement l'adapter pour afficher une autre quantité !

En détail
+++++++++

Un fichier JSON commence par :code:`{` et se termine par :code:`}`. Entre les accolades se trouvent les données qui peuvent être :

1. Une paire clé/valeur, ex. :code:`"method" : "jacobi"`
2. Une liste ordonnée de valeurs : :code:`"resvec" : [ 12.1, 8.2, 5.4, 3.2]`

Une clé peut aussi faire référence à un objet, entouré alors d'accolades, comme :code:`"jacobi"` ci-dessous qui regroupe plusieurs paramètres :

.. code-block:: json

  {
    "jacobi": {
      "method": "jacobi",
      "N": 4,
      "ntier": 3,
      "resvec":  [ 12.1, 8.2, 5.4, 3.2],
      "cpu_time": 3.2323,
    }
  }

Nous pouvons concaténer ainsi plusieurs résultats de simulations dans un même fichier :

.. code-block:: json

  {
    "jacobi": {
      "method": "jacobi",
      "N": 4,
      "ntier": 4,
      "resvec":  [ 12.1, 8.2, 5.4, 3.2],
      "cpu_time": 3.2323
    },
    "gauss": {
      "method": "gauss",
      "N": 4,
      "ntier": 2,
      "resvec":  [ 6.1, 2.2],
      "cpu_time": 1.397
    }
  }

.. proof:remark::

  Selon les lecteurs JSONS, il n'est pas nécessaire que le dernier élément se termine par une virgule :code:`,`. Cependant, en Python, il faut éviter de placer une virgule à chaque dernier élément d'une liste.

JSON : Input/Output (I/O)
-------------------------

Nous utilisons un parser JSON pour le C++ `disponible sur Github <https://github.com/nlohmann/json>`_. Très simple à utiliser, vous devez uniquement télécharger `le fichier *header* json.hpp <https://raw.githubusercontent.com/nlohmann/json/develop/single_include/nlohmann/json.hpp>`_ et le placer dans votre dossier :code:`include`. Ensuite, il vous suffit de l'inclure via

.. code-block:: cpp

  #include "json.hpp"
  // for convenience
  using json = nlohmann::json;

Les opérateurs de flux entrant et sortant, il est très facile d'écrire sur disque et de lire un tel fichier.

.. proof:remark::

  Du fait de notre configuration, **vous ne pouvez pas inclure** :code:`json.hpp` dans **un fichier source** :code:`.cpp` placé dans le dossier :code:`src` ! En effet cela pourrait amener à une erreur de compilation (multiple définitions de fonctions) : le fichier :code:`json.hpp` sera compilé autant de fois qu'il sera appelé...

  Réservez :code:`json.hpp` **uniquement** pour vos fichiers principaux (contenant la fonction :code:`main()`).

Écriture : exemple
++++++++++++++++++

.. code-block:: cpp

  #include <iomanip> // Pour std::setw(2)
  #include <fstream>
  #include <vector>
  #include "json.hpp"

  // for convenience
  using json = nlohmann::json;

  int main(){
    //Jacobi
    double cpu_jac = 3.2323;
    std::vector<double> resvec_jac({12.1, 8.2, 5.4, 3.2});
    // create an empty structure (null)
    json j;
    j["jacobi"]["method"] = "jacobi";
    j["jacobi"]["N"] = 4;
    j["jacobi"]["niter"] = 4;
    j["jacobi"]["resvec"] = resvec_jac;
    j["jacobi"]["cputime"] = cpu_jac;
    // Gauss
    double gauss_cpu = 1.397;
    std::vector<double> resvec_gauss({6.1, 2.2});
    j["gauss"]["method"] = "gauss";
    j["gauss"]["N"] = 4;
    j["gauss"]["niter"] = 2;
    j["gauss"]["resvec"] = resvec_gauss;
    j["gauss"]["cputime"] = gauss_cpu;

    std::ofstream f("data.json");
    // std::setw(2) impose tabulation = 2 espace (plus joli)
    f << std::setw(2)<< j;
    f.close();
    return 0;
  }

Le résultat est alors celui souhaité plus haut :

.. code-block:: json

  {
    "gauss": {
      "N": 4,
      "cputime": 1.397,
      "method": "gauss",
      "niter": 2,
      "resvec": [
        6.1,
        2.2
      ]
    },
    "jacobi": {
      "N": 4,
      "cputime": 3.2323,
      "method": "jacobi",
      "niter": 4,
      "resvec": [
        12.1,
        8.2,
        5.4,
        3.2
      ]
    }
  }

Lecture : exemple
+++++++++++++++++

En général, nous ne lirons pas les fichiers en `C++` mais en Python avec `Matplotlib <https://matplotlib.org/>`_ comme expliqué ensuite. Cependant, la lecture d'un fichier avec ce parser JSON est aisée :

.. code-block:: cpp

  #include <iomanip> // Pour std::setw(2)
  #include <iostream>
  #include <fstream>
  #include "json.hpp"

  // for convenience
  using json = nlohmann::json;

  int main(){
    json j;
    std::ifstream f("data.json");
    f >> j;
    f.close();
    return 0;
  }
