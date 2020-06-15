# ITI 1121 - Lab 06

## Submission

Please read the [junit instructions](JUNIT.en.md) for help
running the tests for this lab.

Please read the [Submission Guidelines](SUBMISSION.en.md) carefully.
Errors in submitting will affect your grades.

Submit your answers to

* ArrayStack.java
* Dictionary.java
* DynamicArrayStack.java
* Pair.java
* Map.java
* Stack.java


# Part 1: Stack

## Learning objectives

* **Recognize** the situation where a stack would be necessary for the conception of an algorithm
* **Describe** the implementation of a stack using an array
* **Implement** a stack using a dynamic array
* **Implement** a predefined interface type
* **Apply** the concept of generic types for the implementation of classes and interfaces


### Introduction

**Stacks** are part of the common data structure types used by programmers. There are however many ways to implement them. To do so, you must understand their behaviors. First, stacks follow the principle of **last-in first-out** (LIFO). In other words, the elements are stacked one on top of the other, and you can only access the element on top of the stack. To access an element below the top element, you can only access this element by removing every element on top of it, i.e. entered after.

You are constantly surrounded by stacks in the real world. Determine if these are indeed an example of a stack (true) or not (false).

##### Question 1.1 :

Tennis balls in their container

##### Question 1.2 :

The waiting list for students to be admitted in engineering.

##### Question 1.3 :

A stack of dishes.


##### Question 1.4 :

Evaluation of postfix expressions.

##### Question 1.5 :

The chips in a _Pringles_ can

##### Question 1.6 :

A toll booth

##### Question 1.7 :

The back button in your web browser

### Application

This part of the lab will focus on two of the three implementations of the interface **Stack**. You will be evaluated on these two implementations in the midterm evaluation.

#### 1.1 Modify the interface Stack: add a method clear()

Modify the interface Stack bellow to add a method abstract public void clear().

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

#### 1.2 Implement the method clear() of the class ArrayStack

The class **ArrayStack** uses an array of fixed size and implements the interface Stack. Since the interface **Stack** has been modified to have the method **clear()**, the implementation of the class **ArrayStack** is faulty. (Try to compile it now without changing anything, what error do you see?).
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

Since the class **ArrayStack** implements the interface Stack, it needs to have an implementation for all methods of the interface. Therefore, you will need to write a method **void clear()**. This method will remove all the elements from this stack (ArrayStack). The stack will be empty after this call. Use the class **L6Q1** to test the implementation of the method **clear()**.

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

#### 1.3 Create a new class DynamicArrayStack

Modify the class **ArrayStack** using the technique known as dynamic array. This new class **DynamicArrayStack** has a constant _DEFAULT_INC_, with the value 25. The constructor takes an argument _capacity_ which is the initial size of the array. However, the minimum size of the array is never less that DEFAULT_INC (25). It has a getter, getCapacity(), that returns an integer, which is the length of the array (physical size of the stack). When the array becomes full, a new array is created with _DEFAULT_INC_ more cells than the previous array, and filled with the elements from the previous array. Similarly, when the stack has _DEFAULT_INC_ elements less than the length of the array (physical size of the stack), it needs to be automatically reduced in size. Make the necessary changes, particularly in **pop**, **push** and **clear**, and in the **constructor** (recall, the minimal size of an array is _DEFAULT_INC_).

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


## PART 2. Postfix Expressions

This part is for you to do on your own at home, and does not need to be handed in. However, as this material is fair game for the midterm exam, we strongly recommend that you do indeed go through it.

### Introduction

Postfix expressions are an example of the application of a stack. As viewed in class, it is a method where we push the operands of the expression until we encounter an operator. At this point, we remove (**pop**) the two previous elements (the operands). We then do the operation and **push** back the result onto the stack. At the end of the expression we should only be left with one element in our stack.

Let’s look at the postfix expression: **5 2 - 4 \***

1.  First, we read the expression from left to right. We start with numbers that will be used as operands, so we stack them:**Bottom [5 2**
2.  When we encounter an operator (here “-“), we pop the 2 operator from the stack, 2 and 5 respectively. We calculate the result of the operation. Note that the order is important, we take the second element that was removed (5) minus (-) the first element to be removed (2). We then push the result on the stack(3). Don’t forget that 5 and 2 have been removed. **Bottom [3**
3.  The next element is “4”, as it is a number, we push it on the stack: **Bottom [3 4**
4.  Finally we read “*”. As it is an operator (multiplication), we remove the two elements on top of the stack, 4 and 3 respectively. We then do the operation “3 * 4” and we push back the result: **Bottom [12**
5.  We have used all of the elements from our postfix expression and we only have one value in our stack. “12 “ is therefore the answer to the expression!

**Note**: if you encounter an operator but there are not two numbers in the stack, the postfix expression is invalid. Similarly, the expression is invalid if after its execution there is more than one number in the stack.

#### Let’s practice! These concepts might be part of your midterm!

Here is a series of simple exercises. Make sure to understand how to read the postfix expressions and how to convert arithmetic expressions into postfix expression.

##### Question 2.1 :

Convert this expression into a **postfix expression**: 5 * 3 + 5

##### Question 2.2 :

Convert this expression into a **postfix expression**: 4 - 6 / 3 * 2

##### Question 2.3 :

**Solve** this postfix expression (find the result): 9 2 * 6 / 2 3 * +

##### Question 2.4 :

**Solve** this postfix expression (find the result): 4 2 8 * - + 7 +


### Application

#### 2.1 Algo1

For this part of the laboratory, there are two algorithms for the validation of expressions containing parenthesis (round, squared and brackets).

The class **Balanced** below presents the first algorithm **algo1**, a simple version to validate expressions. We see that a well formed expression is one that for each type of parenthesis (round, squared and curly brackets), the number of opening parenthesis is equal to the number of closing parenthesis.

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

Compile this program and experiment. First make sure that the program runs properly for valid expression such as “()[]()”, “([][()])”. You will remark that the algorithm works for expressions that contains operands and operators: “(4 * (7 - 2))”.

Next you will need to find expressions where the algorithm returns true even if the expressions are not valid.

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

You found expressions that make this algorithm fail? Good job! Indeed, a valid expression is one where the number of opening parenthesis is equal to the number of closing ones, for each type of parenthesis. But also, when we encounter a closing parenthesis, it has to be the same type as the last opening parenthesis that wasn’t treated. For instance, “ (4 + [2 - 1)] " is not valid!

You need to implement an algorithm using a stack to validate expressions: return **true** if the expression is valid, **false** otherwise. Also, the analysis should go through the stack only once. You will need to create your implementation in the class **Balanced**, name this method **algo2**. (the model solution is about 15 lines!)

Do several tests using valid and invalid expressions. Make sure that your algorithm treats this case: "((())". How do you deal with this case?


## PART 3. Generic Data Structures

It is possible to use **generic types** to implement classes or interfaces. These can be reused in many contexts, with parameterized types. Remember the interface Comparable! By using the generic type, we make sure to keep the **compiler validation**.


#### 3.1 Implement the interface Map

Define the interface **Map**. A class that realizes Map has to save the **key-value** association. In our case, we need a Map to contain identical keys, in which case the method get, put, replace, and remove will refer to the association containing this key that is the further right (the last). Map is a generic class with two types of parameters, K and V, where K is the type of the Key and V is the type of the values. A Map has the following methods:

1.  **V get(K key)** : Returns the rightmost value associated to the key specified in the parameter.
2.  **boolean contains(K key)** : Returns true if there is an association for the key specified in the parameter
3.  **void put(K key, V value)** : Creates a new **key-value** association
4.  **void replace(K key, V value)** : Replaces the value of the rightmost occurrence of the association for the specified key
5.  **V remove(K key)** : Removes the rightmost occurrence of the specified key and returns the value that was associated to this key

**Note** that no parameter can be null in any of these methods
* Map.java

#### 3.2 Implement the class Dictionary

**Dictionary** keeps track of **String-Integer** associations (key-value).

* The **Dictionary** class implements the interface **Map <String,Integer >**.
* It uses an array (the first instance variable) to store each association. The elements in this array are of type **Pair** , a class that will be public. A **Pair** type object must store an association, "key" and "value" of type **String** and **Integer** respectively. Study the pre-solved file provided. _Note this is an alternative solution until nested classes are taught in class._
* You must treat your array of items as a stack. So the bottom of your stack will be element 0, while the top of the stack will be the non-null element at the highest position. Use a counter (the second instance variable) that will tell you how many items are in your table.
* Finally, because the **Dictionary** class stores an arbitrarily large number of associations, you will need to use the **dynamic table** technique (as shown in part 1). The **initial size** of the array will be 10, while its **increment** will be 5 whenever the array is full. These two values ​​will be treated as constants.
* You clearly need to implement **all methods** of the interface. Do not forget to consider that the **rightmost** association (element of type Pair) will always be the **top most** of the stack! You will therefore have to go through your table in the opposite direction.
* Add a **toString()** method that prints your table elements from last to first (top of stack to bottom of stack).
* It has getters, getCount() which returns the current number of items in the dictionary, and getCapacity() which returns the current length of the array.
* DictionaryTest.java contains a number of tests for your implementation that you can use while developing your solution.

Note that you do not have to handle exceptional inputs in your solution (for example, removing, getting, or replacing a key that doesn't exist.)

* Dictionary.java
* Pair.java


### Resources

* [https://docs.oracle.com/javase/tutorial/getStarted/application/index.html](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
* [https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)
