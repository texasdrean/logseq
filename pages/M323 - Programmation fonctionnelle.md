- La programmation fonctionnelle est un paradigme de programmation qui insiste sur l'évaluation d'appel de `fonctions` plutôt que sur l'utilisation de variables et de blocs imbriqués
- Les fonctions fonctionnelles sont pures, elles ne modifient pas l'état du monde extérieur rendant les programmes fonctionnels plus faciles à comprendre, à tester et à déboguer
- [Cheat sheet](https://ingin.fr/Cheat_sheet_Functional_Programming.pdf)
- [Functional programming in Javascript](https://ingin.fr/blog/2021/02/13/programmation-fonctionnelle-en-javascript)
- [Functionnal programming in Python](https://docs.python.org/fr/3/howto/functional.html#built-in-functions)
- ----
- `Avantages`
	- - Programmes plus faciles à comprendre et à maintenir que les programmes impératifs
	- - Programmes moins susceptibles d'avoir des bugs
	- - Programmes plus faciles à paralléliser et à distribuer (cache)
- Différence programmation `fonctionnelle` et `impérative`
	- La programmation impérative traite les calculs comme une séquence d'instructions qui modifient l'état du monde extérieur
	- La programmation fonctionnelle traite les calculs comme des évaluations de fonctions qui ne modifient pas l'état du monde extérieur
- ----
- `Termes`
	- ![Screenshot 2023-08-22 at 14.38.28.png](../assets/Screenshot_2023-08-22_at_14.38.28_1692708080295_0.png)
- `Principaux langages`
	- ![Screenshot 2023-08-22 at 14.38.45.png](../assets/Screenshot_2023-08-22_at_14.38.45_1692708101582_0.png)
- ----
- ### Fonction pure
	- Ne modifie pas l'état du monde extérieur -> renverra toujours le même résultat avec les mêmes arguments
	- Pour qu'une fonction soit pure, elle doit respecter 3 conditions:
		- - `Sans effet secondaire` -> ne modifie pas une variable globale, d'instance ou locale
		- - `Déterministe` -> renverra toujours le même résultat avec les mêmes arguments
		- - `Transparence référentielle` -> peut être remplacée par son résultat sans changer le comportement du programme
	- Avantages:
		- - `Facilité de test` -> faciles à tester car comportement bien défini
		- - `Facilité de débogage` -> faciles à déboguer car n'ont pas d'effet secondaire
		- - `Parallélisation` -> faciles à paralléliser car ne dépendent pas de l'état du monde extérieur
		- - `Fiabilité` -> plus fiables car ne peuvent pas provoquer d'erreurs dues à des effets secondaires
	- ```python 
	  def sum(a, b):
	    return a + b
	  ```
- ### Récursivité
	- [Recursion.vercel.app](recursion.vercel.app)
	- La récursivité est une technique consistant à une fonction à s'appeler elle-même.
	- Le `pas récursif` est la partie de l'algorithme qui appelle la fonction elle-même. Il est responsable de résoudre le problème en général.
	- La `clause de finitude` est la partie de l'algorithme indiquant à la fonction d'arrêter de s'appeler elle-même. Cela est nécessaire pour éviter une boucle infinie.
	- ```python 
	  def factorial(n):
	    # Clause de finitude
	    if n == 1 or n == 0:
	      return 1
	    # Pas récursif
	    else:
	      return n * factorial(n - 1)
	  ```
- ### Fonctions lambda
	- Une expression lambda est une fonction anonyme, déclarée sans être associée à un nom.
	- C'est un outil puissant pouvant être utilisé dans de nombreux contextes différents, notamment:
		- - rendre le code plus concis et lisible
		- - passer des fonctions en tant qu'arguments à d'autres fonctions
		- - Créer des fonctions anonymes qui seront utilisées 1x
	- ```python 
	  # Déclaration d'une fonction lambda
	  sum = lambda a, b: a + b
	    
	  # Appel de la fonction lambda
	  print(sum(1, 2)) # 3
	  ```
- ### Curryfication
	- La curryfication consiste à transformer une fonction qui prend plusieurs arguments en une séquence de fonctions, chacune avec 1 seul argument -> ça permet de diviser une fonction en plusieurs fonctions plus petties et plus spécialisées
	- La curryfication permet de:
		- - rendre le code plus lisible et plus facile à comprendre
		- - réutiliser des fonctions de manière plus efficace
		- - créer des fonctions plus flexibles
	- ```python 
	  # Fonction non currifiée
	  def add(a, b):
	    return a + b
	  
	  # Fonction currifiée
	  def add_one(a):
	    return add(a, 1)
	  
	  # Utilisation
	  print(add_one(5)) # 6
	  ```
- ### Fonctions de première classe
	- Fonction qui peut être traitée comme n'importe quelle autre valeur. Càd qu'elle peut être stockée dans une variable, passée en tant qu'argument à une autre fonction, ou être retournée par une autre fonction
	- ```python 
	  def my_function(name):
	    print("Hello, " + name)
	    
	  # Stocker la fonction dans une variable
	  my_function_variable = my_function
	  
	  # Passer la fonction en tant qu'argument à une autre fonction
	  def my_other_function(my_function):
	    my_function("John Doe")
	    
	  my_other_function(my_function_variable)
	  ```
- ### Fonctions d'ordre supérieur
	- Fonction qui peut prendre d'autres fonctions en arguments ou retourner une autre fonction. C'est un concept important en programmation fonctionnelle, utilisées pour rendre le code plus concis et lisible.
	- ```python 
	  def apply_function(function, number):
	    return function(number)
	  
	  def square(number):
	    return number * number
	  
	  print(apply_function(square, 5)) # 26
	  ```
	- #### Méthode `map`
		- ```python 
		  # Prend une fonction et une séquence en entrée et renvoie une nouvelle séquence
		  # contenant le résultat de l'application de la fonction à chaque élément de
		  # la séquence
		  numbers = [1, 2, 3, 4, 5]
		  
		  squares = list(map(lambda number: number * number, numbers))
		  
		  print(squares) # [1, 4, 9, 16, 25]
		  ```
	- #### Méthode `filter`
		- ```python
		  # Prend une fonction et une séquence en entrée et renvoie une nouvelle séquence
		  # contenant uniquement les éléments de la séquence pours lesquels la fonction
		  # renvoie la valeur true
		  numbers = [1, 2, 3, 4, 5]
		  
		  even_numbers = list(filter(lambda number: number % 2 == 0, numbers))
		  
		  print(even_numbers) # [2, 4]
		  ```
	- #### Méthode `reduce`
		- ```python 
		  # Prend une fonction et une séquence en entrée et renvoie une valeur unique
		  # obtenue en appliquant la fonction de manière récursive à la séquence.
		  # L'argument initial_value est la valeur initiale de l'accumulateur
		  # La fonction accumulator est appelée à chaque itération de la boucle. Elle doit
		  # renvoyer la valeur de l'accumulateur après l'application de la fonction à 
		  # l'élément actuel de la séquence
		  import functools
		  
		  sum = functools.reduce(lambda accumulator, element: accumulator + element, numbers, 0)
		  
		  print(sum) # 15
		  ```
	-