# ITI 1121 - Lab 06

## Soumission

Veuillez lire les [instructions junit](JUNIT.fr.md) pour obtenir de l'aide avec l'exécution des tests pour ce laboratoire.

Veuillez lire attentivement les [Directives de soumission](SUBMISSION.fr.md). Les erreurs de soumission affecteront vos notes.


Soumettez les réponses aux

* ArrayStack.java
* Dictionary.java
* DynamicArrayStack.java
* Pair.java
* Map.java
* Stack.java


# Première partie: Stack

## Objectifs d'apprentissage

* **Reconnaître** les situations où une pile est nécessaire pour la conception d'un algorithme.
* **Décrire** l'implémentation d'une pile à l'aide d'un tableau.
* **Implémenter** une pile à l'aide d'un tableau dynamique.
* **Implémenter** un type interface prédéfini.
* **Appliquer** le concept de type générique pour l'implémentation de classes et d'interfaces.


### Introduction

Les **piles** sont couramment en programmation. Dan un premier temps, assurez-vous de bien comprendre leur utilisation. Les **piles** respectent le protocole **last-in first-out** ou **LIFO** (dernier entré premier sorti). Cette structure de données empile les éléments les uns par dessus les autres. Vous n'avez accès qu'à l'élément du dessous.

Voici des exemples de la vie courante. Déterminez s'il s'agit d'exemple valable (vrai) où une pile est utilisée ou non (faux).

##### Question 1.1 :

Les balles de tennis dans leur contenant

##### Question 1.2 :

Les étudiants sur la liste d’attente d’admission en génie

##### Question 1.3 :

Une pile d’assiettes dans votre armoire

##### Question 1.4 :

La gestion des tâches pour une imprimante

##### Question 1.5 :

L’évaluation d’expressions postfixes

##### Question 1.6 :

Un contenant de chips Pringles

##### Question 1.7 :

Un poste de péage

##### Question 1.8 :

Le bouton «Précédent» dans votre fureteur (« Browser »)


### Application

Cette partie du laboratoire porte sur deux des trois implémentations de l'interface Stack. Il s'agit de deux implémentations lesquelles vous pourriez être évalué à l'examen de mi-session.

#### 1.1 Modifier l'interface Stack : ajout d'une méthode clear()

Modifiez l'interface Stack ci-bas afin d'y ajouter la méthode abstraite public void clear().

```java
public interface Stack<E> {
  boolean isEmpty();
  E peek();
  E pop();
  void push(E element);
}
```

```java
public interface Stack<E> {
  public abstract boolean isEmpty();
  public abstract E peek();
  public abstract E pop();
  public abstract void push( E element);
  /* Add clear */
}
```

###### File:

*  Stack.java

#### 1.2 Implémenter la méthode clear() de la classe ArrayStack

La classe **ArrayStack** utilise un tableau de taille fixe et réalise l'interface **Stack**. Puisque l'interface **Stack** a été modifiée afin d'y ajouter la méthode **clear()**, l'implémentation de la classe **ArrayStack** est défectueuse (essayez d'abord de la compiler sans y apporter de changement, quel message d'erreur est affiché à l'écran ?).
```java
public class ArrayStack<E> implements Stack<E> {

  private E[] elems;  // Used to store the elements of this ArrayStack
  private int top;    // Designates the first free cell
  private int capacity;    // Designates the capacity of the Array

  @SuppressWarnings( "unchecked" )

  // Constructor

  public ArrayStack( int capacity ) {
    elems = (E[]) new Object[ capacity ];
    top = 0;
    this.capacity = capacity;
  }

  // Returns true if this ArrayStack is empty

  public boolean isEmpty() {
    // Same as:
    // if ( top == 0 ) {
    //     return true;
    // } else {
    //     return false;
    // }

    return ( top == 0 );
  }

  // Returns the top element of this ArrayStack without removing it

  public E peek() {
    // pre-conditions: ! isEmpty()
    return elems[ top-1 ];
  }

  // Removes and returns the top element of this stack

  public E pop() {
    // pre-conditions: ! isEmpty()

    // *first* decrements top, then access the value!
    E saved = elems[ --top ];

    elems[ top ] = null; // scrub the memory!

    return saved;
  }

  // Puts the element onto the top of this stack.

  public void push( E element ) {
    // Pre-condition: the stack is not full
    // *first* stores the element at position top, then increments top

    elems[ top++ ] = element;
  }


  // Gets current capacity of the array (for testing purpose)
  public int getCapacity() {
    return elems.length;
  }


  @SuppressWarnings( "unchecked" )

  // Add clear method.

}
```

Puisque la classe **ArrayStack** réalise l'interface Stack, elle doit fournir une implémentation pour toutes les méthodes de l'interface. Ainsi, vous devez écrire une méthode **void clear()**. Cette dernière retire tous les éléments de cette pile (**ArrayStack**). La pile sera vide suite à cet appel. Utilisez la classe **L6Q1** pour tester votre implémentation de la méthode clear.

```java
public class L6Q1 {

  public static void main( String[] args ) {

    Stack<String> s;

    s = new ArrayStack<String>( 10 );

    for ( int i=0; i<10; i++ ) {
      s.push( "Elem-" + i );
    }

    s.clear();

    while ( ! s.isEmpty() ) {
      System.out.println( s.pop() );
    }

    for ( int i=0; i<10; i++ ) {
      s.push( "** Elem-" + i );
    }

    while ( ! s.isEmpty() ) {
      System.out.println( s.pop() );
    }

  }
}
```

If you compile and run this class, the output will be:

```java
Elem-9
Elem-8
Elem-7
Elem-6
Elem-5
Elem-4
Elem-3
Elem-2
Elem-1
Elem-0
```

###### Files:

* ArrayStack.java
* L6Q1.java

#### 1.3 Créer une nouvelle classe DynamicArrayStack

Modifier la classe **ArrayStack** afin qu'elle utilise la technique de **tableau dynamique**. Cette nouvelle classe, **DynamicArrayStack**, possède une constante nommée DEFAULT_INC dont la valeur est 25. Le constructeur prend un argument, capacity, qui est la taille initiale du tableau (ont dit aussi taille physique de la pile). Cependant, la taille physique (longueur du tableau) n'est jamais moins que DEFAULT_INC (donc 25). Cette classe contient aussi une méthode d'accès (getter), **getCapacity()**, qui retourne un entier représentant la longueur du tableau (taille physique). Lorsque votre tableau est plein, un nouveau tableau ayant DEFAULT_INC cellules de plus que la taille actuelle du tableau doit être créée. Vous devez y copier les éléments de l'ancien tableau. Par ailleurs, à l'inverse, lorsque la taille logique de la pile (nombre d'éléments) a DEFAULT_INC éléments de moins que la taille physique de la pile (taille du tableau), vous devez automatiquement en diminuer la taille (le nouveau tableau a alors DEFAULT_INC cellules de moins que l'ancien tableau). Faites les ajustements nécessaires, notamment dans **pop, push, clear** et les constructeurs (rappel, la taille physique (taille du tableau) minimale est DEFAULT_INC).

```java
public class DynamicArrayStack<E> implements Stack<E> {

  // Instance variables

  private E[] elems;  // Used to store the elements of this ArrayStack
  private int top;    // Designates the first free cell
  private static final int DEFAULT_INC = 25;   //Used to store default increment / decrement

  @SuppressWarnings( "unchecked" )

  // Constructor
  public DynamicArrayStack( int capacity ) {
    // Your code here.
  }

  // Gets current capacity of the array
  public int getCapacity() {
    return elems.length;
  }

  // Returns true if this DynamicArrayStack is empty
  public boolean isEmpty() {
    return ( top == 0 );
  }

  // Returns the top element of this ArrayStack without removing it
  public E peek() {
    return elems[ top-1 ];
  }

  @SuppressWarnings( "unchecked" )

  // Removes and returns the top element of this stack
  public E pop() {
    // Your code here.
  }

  @SuppressWarnings( "unchecked" )

  // Puts the element onto the top of this stack.
  public void push( E element ) {
    // Your code here.
  }

  @SuppressWarnings( "unchecked" )

  public void clear() {
    // Your code here.
  }

}
```

###### File:

* DynamicArrayStack.java


## Deuxième partie. Les expressions postfixes

Cette partie est laissée à votre propre discrétion et n’a pas besoin d’être remise. Toutefois, comme cette matière est peut être couverte à l’examen de mi-session, nous vous recommandons fortement de la parcourir.

### Introduction

Les expressions postfixes sont un exemple d'application des piles. Tel que vu en classe, il s'agit d'une méthode où l'on empile (**push**) les opérandes de l'expression jusqu'à ce que l'on rencontre un opérateur. À ce moment, on retire (**pop**) les deux éléments précédents, des opérandes. On effectue l'opération et on empile ensuite le résultat dans la pile avec les autres opérandes. À la fin de la lecture de l'expression, il ne devrait nous rester qu'un élément, soit un nombre dans notre pile.

Prenons l'expression postfixe: **5 2 - 4 \***

1.  D'abord, on lit l'expression de gauche à droite. Il s'agit de nombres qui serviront d'opérandes, donc on les empile:**Bottom [5 2**
2.  À la rencontre d'un opérateur, ici « - », on retire 2 opérandes de la pile 2 et 5 respectivement. On effectue l'opération. Notez que l'**ordre est important**, le second élément retiré « 5 » moins « - » le premier élément retiré « 2 ». On empile ensuite le résulat « 3 » à la pile (n'oubliez pas que 5 et 2 ont été retirés). **Bottom [3**
3.  Le prochain élément est « 4 ». Comme il s'agit d'un nombre, on l'empile: **Bottom [3 4**
4.  Enfin, on lit « * ». Comme il s'agit d'un opérateur (multiplication), on retire les deux éléments du dessus, 4 et 3 respectivement. On effectue l'opération « 3 * 4 » et on empile le résultat. **Bottom [12**
5.  Nous avons utilisé tous les éléments de notre expression postfixe et il ne nous reste qu'une valeur dans notre pile. « 12 » est donc le résulat de notre expression!

**Note**: Si vous rencontrez un opérateur dans votre expression et qu'il n'y a pas 2 nombres (opérandes) sur le dessus de votre pile, l'expression postfixe est invalide. Il va de même si vous avez plus d'un nombre dans votre pile à la fin de la lecture de votre expression!

#### Pratiquez-vous! Ce concept peut faire partie de votre examen de mi-session!

Voici une série d'exercices simples. Assurez-vous de bien comprendre comment lire des expressions postfixes et aussi de convertir une expression arithmétique en expression postfixe.

##### Question 2.1 :

Convertissez l'expression suivante en **expression postfixe**: 5 * 3 + 5

##### Question 2.2 :

Convertissez l'expression suivante en **expression postfixe**: 4 - 6 / 3 * 2

##### Question 2.3 :

**Évaluez** l'expression postfixe suivante (trouvez le résultat): 9 2 * 6 / 2 3 * +

##### Question 2.4 :

**Évaluez** l'expression postfixe suivante (trouvez le résultat): 4 2 8 * - + 7 +


### Application

#### 2.1 Algo1

Pour cette partie du laboratoire, il y a deux algorithmes pour la validation d'expressions contenant des parenthèses (rondes, frisées et carrées).

La classe **Balanced** ci-dessous présente le premier algorithme **algo1**, une version simple pour valider des expressions. On remarque qu'une expression bien formée est telle que pour chaque type de parenthèses (les rondes, les frisées et les carrées), le nombre de parenthèses ouvrantes est égal au nombre de parenthèses fermantes. D'où l'algorithme suivant :

```java
public static boolean algo1(String s) {

  int curly, square, round;

  curly = square = round = 0;

  for (int i=0; i < s.length(); i++) {

    char c;
    c = s.charAt(i);

    switch (c) {
      case ’{’:
        curly++;
        break;
      case ’}’:
        curly--;
        break;
      case ’[’:
        square++;
        break;
      case ’]’:
        square--;
        break;
      case ’(’:
        round++;
        break;
      case ’)’:
        round--;
    }
  }

  return curly == 0 && square == 0 && round == 0;
}
```

Compilez ce programme et expérimentez. D'abord, assurez-vous qu'il fonctionne pour des expressions valides, telles que “()[]()”, “([][()])”. Vous remarquerez que l'algorithme fonctionne aussi pour des expressions qui contiennent des opérandes et des opérateurs : “(4 * (7 - 2))”.

Ensuite, vous devez trouver des expressions pour lesquelles l'algorithme retourne true bien que ces expressions ne sont pas bien formées.

```java
public class Balanced {

  public static boolean algo1( String s ) {

    int curly = 0;
    int square = 0;
    int round = 0;

    for ( int i=0; i<s.length(); i++ ) {

      char c = s.charAt( i );

      switch ( c ) {
      case '{':
        curly++;
        break;
      case '}':
        curly--;
        break;
      case '[':
        square++;
        break;
      case ']':
        square--;
        break;
      case '(':
        round++;
        break;
      case ')':
        round--;
      }
    }
    return curly == 0 && square == 0 && round == 0;
  }

  public static void main( String[] args ) {
    for ( int i=0; i<args.length; i++ ) {
      System.out.println( "algo1( \"" + args[ i ] + "\" ) -> " + algo1( args[ i ] ) );
    }
  }
}
```

###### File

* Balanced.java

#### 5. Algo2

Vous avez trouvé des expressions qui font échec à cet algorithme. Très bien ! En effet, une expression bien formée est une expression telle que le nombre de parenthèses ouvrantes et fermantes est le même, et ce, pour chaque type de parenthèses. Mais aussi, lorsqu’on lit une telle expression de gauche à droite et que l’on rencontre une parenthèse fermante alors son type doit être le même que celui de la dernière parenthèse ouvrante rencontrée qui n’a pas encore été traitée (associée). Par exemple, « (4 + [2 - 1)] » n'est pas valide!

Vous devez implémenter un algorithme à base de pile afin de valider des expressions : retourne **true** si l’expression est bien formée et **false** sinon. De plus, l’analyse ne devrait parcourir la chaîne qu’une seule fois. Vous devez créer votre implémentation dans la class Balanced, nommez cette méthode **algo2**. (La solution modèle a 15 lignes !)

Faites plusieurs tests à l’aide d’expressions valides et non valides. Assurez-vous que votre algorithme traite ce cas-ci : “((())” ? Comment traitez-vous ce cas-ci ?


## Troisième partie. Structures de données génériques

Les interfaces et les classes peuvent avoir un paramètre de type. On parle alors de **types génériques**. Ces types peuvent être utilisées dans plusieurs contextes. Rappelez-vous de l'interface Comparable! Les types génériques sont à la fois versatiles et sécuritaires.


#### 3.1 Implémenter l'interface Map

Définir l'interface **Map**. Une classe qui réalise l'interface **Map** doit sauvegarder des associations clé-valeur (key-value). Ici, nous souhaitons qu’un objet **Map** puisse contenir plusieurs clés (key) identiques, auquel cas les méthodes **get, put, replace** et **remove** feront référence à la l’association contenant cette clé se trouvant la plus à droite (la plus récente). **Map** possède deux paramètres de type, **K **et **V**, où **K** est le type des clés (keys) et V le type des valeurs (values). L'interface Map possède les méthodes suivantes :

1.  **V get(K key)** : retourne la **valeur** la plus à droite (rightmost value) associée avec la clé spécifiée en paramètre.
2.  **boolean contains(K key)** : retourne **true** s’il existe une association pour la clé spécifiée en paramètre.
3.  **void put(K key, V value)** : crée une nouvelle association **key-value**.
4.  **void replace(K key, V value)** : remplace la **valeur** associée à l’association la plus à droite possédant la clé spécifiée en paramètre (replaces the value of the rightmost occurrence of the association for the specified key)
5.  **V remove(K key)** : retire **l’association** la plus à droite possédant la clé spécifiée et retourne la valeur qui était associée à cette clé.

**Notez** qu’aucun paramètre ne peut être **null** dans chacune de ces méthodes.
* Map.java

#### 3.2 Implémenter la classe Dictionary

**Dictionary** garde la trace des associations **String-Integer** (key-value).

* La classe **Dictionary** réalise (implements) **l’interface Map <String,Integer >**.
* Elle utilise un tableau (première variable d'instance) pour stocker chaque association. Les éléments de ce tableau sont de type **Pair**. Un objet de type **Pair** doit stocker une association, « key » et « value », de type **String** et **Integer** respectivement. Etudiez le fichier fournit et déjà complété.
* Vous devez traiter votre tableau d'éléments comme une pile. Ainsi, le bas de votre pile sera l'élément 0, tandis que le dessus de la pile sera l'élément non-null à la position la plus élevée. Utilisez un compteur (seconde variable d'instance) qui vous indiquera combien d'éléments sont dans votre tableau.
* Finalement, comme la classe **Dictionary** stocke une quantité arbitrairement large d’associations, vous devrez utiliser la technique de **tableau dynamique** (comme présenté dans la partie 1). La **taille initiale** du tableau sera de 10, tandis que son **incrément** sera de 5 à chaque fois que le tableau est plein. Ces deux valeurs seront traitées comme des constantes.
* Vous devez évidemment implémenter **toutes les méthodes** de l'interface. N'oubliez pas de considérer que l'association (élément de type Pair) **la plus à droite** sera toujours celle qui est la **plus au-dessus** de la pile! Vous devrez donc parcourir votre tableau en sens inverse.
* Ajoutez une méthode **toString** qui imprime les éléments de votre tableau du dernier au premier (haut de la pile au bas de la pile).
* Il contient des getters, getCount () qui renvoie le nombre actuel d'éléments dans le dictionnaire et getCapacity () qui renvoie la longueur actuelle du tableau.
* DictionaryTest.java comprend une série de tests pour votre implémentation.

Note that you do not have to handle exceptional inputs in your solution (for example, removing, getting, or replacing a key that doesn't exist.)

* Dictionary.java
* Pair.java


### Resources

* [https://docs.oracle.com/javase/tutorial/getStarted/application/index.html](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)