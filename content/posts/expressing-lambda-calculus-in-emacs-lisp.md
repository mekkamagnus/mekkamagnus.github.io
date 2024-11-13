+++
title = "Expressing Lambda Calculus in Emacs Lisp"
author = ["Mekael Turner"]
date = 2024-11-12T00:00:00+08:00
tags = ["functional-programming", "monoid", "javascript", "the-lambda-calculus"]
draft = false
+++

The lambda calculus is a foundational mathematical framework for studying functions. It forms the bedrock of functional programming, offering a powerful and elegant way to define and manipulate functions. Emacs Lisp, a dialect of Lisp heavily used within the Emacs editor, offers a flexible environment for exploring these concepts.

In this article, we'll embark on a journey to understand how the lambda calculus is expressed within Emacs Lisp, using illustrative examples to highlight the core concepts.


## Functions as Lambda Expressions {#functions-as-lambda-expressions}

Let's begin by considering a simple function that adds 3 to a given number:

```emacs-lisp
(let ((x 20))
  (+ x 3))
```

This code defines a function using the `let` construct, binding the variable `x` to the value 20 and then performing the addition. We can rewrite this using a lambda expression, a core construct of the lambda calculus:

```emacs-lisp
((lambda (x) (+ x 3)) 20)
```

Here, we define a lambda function `(lambda (x) (+ x 3))`, which takes a single argument `x` and returns the result of adding 3 to it. The argument `20` is then passed to this function, yielding the same result as the previous `let` expression.


## Nested Lambdas: Simulating `let*` {#nested-lambdas-simulating-let}

The `let*` construct in Emacs Lisp allows for sequential binding of variables, where each subsequent variable can refer to previously defined ones. Let's consider an example:

```emacs-lisp
(let* ((x 20)
         (y x)
         )
    x)
```

This code first binds `x` to 20 and then binds `y` to the value of `x`. It then returns the value of `x`. We can mimic this behavior with nested lambda functions:

```emacs-lisp
((lambda (x)
     ((lambda (y) x) x)) 20)
```

The outer lambda binds `x` to 20. The inner lambda binds `y` to the value of `x` but ultimately returns the value of `x`, demonstrating that the binding of `y` doesn't affect the result.


## Currying: A Functional Approach {#currying-a-functional-approach}

The nested lambda functions used above hint at a core concept in functional programming: _currying_. Currying transforms a function that takes multiple arguments into a series of functions, each taking a single argument. While not strictly currying, the nested lambda functions in the previous example illustrate a similar approach.

The outer lambda takes the first argument (`x`) and then returns the inner lambda, which takes the second argument (`y`). While the inner lambda in this example ignores the value of `y`, currying allows us to build up functions incrementally, one argument at a time.


## Conclusion {#conclusion}

Exploring the lambda calculus within Emacs Lisp provides valuable insights into the functional programming paradigm. By understanding how lambda expressions work and how they can be used to express concepts like `let*`, we gain a deeper understanding of the power and elegance of functional programming. As you continue your journey through the world of functional programming, remember that the lambda calculus is a potent tool, offering a unique and powerful way to represent and manipulate functions.
