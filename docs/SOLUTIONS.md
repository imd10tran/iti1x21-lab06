
# Queastion 1 :

**Stacks** are part of the common data structure types used by programmers. There are however many ways to implement them. To do so, you must understand their behaviors. First, stacks follow the principle of **last-in first-out** (LIFO). In other words, the elements are stacked one on top of the other, and you can only access the element on top of the stack. To access an element below the top element, you can only access this element by removing every element on top of it, i.e. entered after.

You are constantly surrounded by stacks in the real world. Determine if these are indeed an example of a stack (true) or not (false).

## Question 1.1 :

Tennis balls in their container

### Solutions

```java
TRUE
```

## Question 1.2 :

The waiting list for students to be admitted in engineering.

### Solutions

```java
FALSE.
```

The first student on the list will be the next one to be admitted.


## Question 1.3 :

A stack of dishes.

### Solutions

```java
TRUE
```

## Question 1.4 :

Evaluation of postfix expressions.

### Solutions

```java
TRUE
```

## Question 1.5 :

The chips in a _Pringles_ can

### Solutions

```java
TRUE
```

## Question 1.6 :

A toll booth

### Solutions

```java
FALSE
```

## Question 1.7 :

The back button in your web browser

### Solutions

```java
TRUE
```

# Queastion 2 :

Here is a series of simple exercises. Make sure to understand how to read the postfix expressions and how to convert arithmetic expressions into postfix expression.

## Question 2.1 :

Convert this expression into a **postfix expression**: 5 * 3 + 5

### Solutions

```java
5 3 * 5 +
```

## Question 2.2 :

Convert this expression into a **postfix expression**: 4 - 6 / 3 * 2

### Solutions

```java
4 6 3 / 2 * -
```

## Question 2.3 :

**Solve** this postfix expression (find the result): 9 2 * 6 / 2 3 * +

### Solutions

```java
9
```

## Question 2.4 :

**Solve** this postfix expression (find the result): 4 2 8 * - + 7 +

### Solutions

```java
Error! This is not a valid postfix expression!
```
