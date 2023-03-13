# Ocaml expressions, values, and types

Some basics of Ocaml. Ocaml is a compiled and bootstrapped language. It is implicitly typed. That means, the compiler infers the type of your variables and values at compile time. Ocaml is also statically typed, meaning once the type of a variable is infered, the variable must abide by the type throughout its scope. Everything in Ocaml is immutable. Everything means everything. Once you initialize a variable, you cannot change throughout its scope. You should redefine it to change it. That being said, `=` is an equality operator and not the assignment operator outside `let` expressions. 

Some primitive built-in data types are `int`, `float`, `char`, `string`, `bool`, and `unit`. Other composite data types include `tuples`, `lists`, `option`, and variants.

We know the primitive data types but we will learn more about the others later down in the discussion. Arithmetic operators in Ocaml are not overloaded. So, you can use `+`, `-`, `*`, `/` on two ints but not on floats. For floats, they are `+.`, `-.`, `*.`, `/.`. **Notice the period**.

Expressions are something that evaluates to some value. Example: `1 + 2`, `2 < 3`, `"hello"`.

## Part 2: Let bindings and Let expressions

Everything in Ocaml is expression , say `e`. So, everything will evaluate to some value of type, say `t`.

Examples:  
- `1: int`
- `true: bool`
- `'e': char`

The `let` syntax is the main way to bind a name to a value. Simply: 

```ocaml
let name = value;; (* syntax *)
let num1 = 5;; (* type: int *)
let num2 = 6;; (* type: int *)
let num3 = num1 + num2;; (* type: int *)
```

We use `let` to create expressions as well. Remember that expressions evaluate to some values. So, the variables initialized in the let expressions are limited to the expression in terms of scope.

Examples:

```ocaml
let x = 8 in x;;  (* will evaluate to 8 *)
let x = 10 in let y = 15 in x + y;; (* nested let expressions *)
let x = 5 in let y = 7 in if x > y then "bigger" else "smaller";; (* expression can be another expression *)
```

## Part 3: Functions and the `rec` keyword

Functions, conventionally, are multiline reusable code that might or might not depend on other variables (arguments). To denote the notion of functions in Ocaml, we can treat the functions as expressions i.e. something that can evaluate to a value. Technically, a function processes the input and generates an output. Putting multiple expressions together can work the same magic. So, we use `let` bindings to bind expression(s) and parameters to some name to make functions.

Example:

```ocaml
let my_function a = a;; (* type 'a -> 'a *)
```
analogous to (java)
```java
<T> T my_function(T a) {
    return a;
}
```

Raising the complexity of the functions:
```ocaml
let my_func param1 param2 = param1 + param2;; (* type: int -> int -> int *)
let to_arr a b = [a; b];;                     (* type: 'a -> 'a -> 'a list *)  
```
analogous to (java)
```java
int my_func(int param1, int param2) {
    return param1 + param2;
}

<T> T[] to_arr(T a, T b) {
    return {a, b};
}
```

```ocaml
let check_empty_string str_param = 
    if str_param = "" then true else false;; (* type: string -> boolean *)

let check_a_string str_param = 
    if str_param = "a" then true else "invalid string";; (* will fail to compile *)
```

Notice how each branch in a function (maybe if-else or pattern matching) should return the same data type.

The general pattern for determining the type of any function is:

`first_param_type -> second_param_type -> ... -> last_param_type -> return_type`.

### Recursive functions

The use of `rec` keyword makes a function recursive. You do not need to make recursive calls, but if you want to, you need the `rec` keyword.

```ocaml
let rec factorial num = 
    if num = 1 then 1 else num * (factorial (num - 1)) (* int -> int *)
```

## Part 4: Lists, Tuples, Variants, and Records

Lists are analogous to arrays to other languages with a difference that the Ocaml lists cannot be indexed. So, recursion is the prime way of iterating over a list and pattern-matching to access an element. The lists are homogenous in nature and the elements are separated by `;`. 

Examples: 
```ocaml
let my_list = [1;2;3];;
let my_second_list param_a param_b = [param_a; param_b] in my_second_list 1 2;;
let my_third_list = "first" :: ["second"; "third"];;
```

You preppend to a list using a cons `::` operator.

Tuples are fixed sized, heterogenous, ordered set of values. You cannot index tuples as well but can use pattern matching to access the desired element.

Examples:
```ocaml
let my_tuple = (1,"string",true);;
let my_second_tuple = [1, 2, "yes, this syntax is also valid"];; (* notice the brackets *)
let my_tuple_func a b c = (a, b, c);; 
```

Variants are beautifications of a certain type of existing data type. It is used to create customs types. Think of it like renaming a particular representation of values.

Examples:
```ocaml
type color = Red | Green | Blue;;
let colors = [Red; Green; Red; Red];;
type linked_list_node = TerminalNode of int | IntermediateNode of int * linked_list_node;;
let d = IntermediateNode(8, IntermediateNode(9, TerminalNode(10)));;
```

In OCaml, records are composite data types that allow you to group several values of different types together under a single name. They are similar to structs in C or classes in object-oriented programming languages.

A record is defined using the `type` keyword, followed by the name of the record and a set of named fields, each with its own type. Here's an example:

Examples:
```ocaml
type person = {
  name : string;
  age : int;
  email : string option;
}
```
In this example, we define a new record type called person that has three named fields: 
`name` of type `string`, `age` of type `int`, and `email` of type `string option`. The `string option` type indicates that `email` may or may not be present, as it is wrapped in an optional type.

Once a record type is defined, you can create instances of it by specifying values for each of its fields:

```ocaml
let alice = { name = "Alice"; age = 25; email = Some "alice@example.com" }
```

Here, we create a new `person` record called `alice` with the values "Alice" for `name`, 25 for `age`, and `Some "alice@example.com"` for `email`.

To access the values of a record, you can use dot notation:

```ocaml
let alice_name = alice.name
```

This assigns the value "Alice" to the variable `alice_name`.

Records are commonly used in OCaml to represent structured data, such as database records or configuration settings. They can be passed as arguments to functions, returned from functions, and even nested inside other records or data structures.

## Part 5: Pattern matching

Pattern matching is like regular expressions for values. You match the values against a desired pattern to validate that value, extract subvalues out of it, or even manipulate the subvalues.

Syntax:
```ocaml
match value with
pattern1 -> code if it match pattern1
| pattern2 -> code if it match pattern2
.
.
| _ -> default code;;
```

- List pattern matching

```ocaml
let my_list [ 1; 2; 3; 4; 5];;

let rec add_one lst = match lst with
[] -> []
| h :: t -> (h + 1) :: (add_one t)
in add_one my_list;;
```

You can even have multiple levels of patterns.

```ocaml
let check_min_len lst = match lst with
[] -> "zero"
| a :: t -> "at least one"
| a :: b :: t -> "at least two"
| a :: b :: c :: t -> "at least three"
| _ -> "at least four";;
```

In the same manner, you can pattern-match a tuple.

```ocaml
let get_nth_element tup index = match tup with
(a, b, c, d) when index = 0 -> a 
| (a, b, c, d) when index = 1 -> b 
| (a, b, c, d) when index = 2 -> c 
| (a, b, c, d) when index = 3 -> d;;

get_nth_element (2, 4, 6, 8) 3;; (* will return 8 *)
```
# Map, Fold, Tree-Type

## Introduction

Today, we'll be covering `map` and `fold`, as well as an example of a custom data type. 

## Part 1: Map

Suppose you have a list in which you want to add one to each element. You could easily write a function to do this:

```ocaml
let rec add1 xs =
  match xs with
      [] -> []
    | h::t -> (h+1)::(add1 t)
;;
```

Now, let's consider a function that squares each element in a list:

```ocaml
let rec square xs =
  match xs with
      [] -> []
    | h::t -> (h*h)::(square t)
;;
```

Notice that both of these functions are *essentially* doing the same thing; they are performing the same task to each element of a list. We can generalize for any function `f` by using `map`.

```ocaml
let rec map f xs = 
  match xs with
      [] -> []
    | h::t -> (f h)::(map f t)
;;
```

## Part 2: Fold

Suppose you want to compute the sum of each element in a list. We can write a function that does this:

```ocaml
let rec sum xs =
  match xs with
      [] -> 0
    | h::t -> h+(sum t)
;;
```

What if you want to compute the size of a list?

```ocaml
let rec size xs =
  match xs with
      [] -> 0
    | h::t -> 1+(size t)
;;
```

In each case, we are keeping track of an *accumulator* and adding onto it based on some property of the current element. We can generalize this to any function `f` using `fold` (also known as `fold_left`) and `fold_right`:

```ocaml
let rec fold f a lst =
    match lst with
    []->a
    |h::t->fold f (f a h) t
;;

let rec fold_right f lst a =
    match lst with
    []->a
    |h::t-> f h (fold_right f t a)
;;
```

A key difference between these two is the order of association. Consider the example of adding all elements of the list `[1;2;3;4]`.  `fold_left` will associate from the left as follows:

`(((0 + 1) + 2) + 3) + 4`

On the other hand, `fold_right` will associate from the right as follows:

`1 + (2 + (3 + (4 + 0)))`

Notice how we assume that our accumulator starts with 0.

## Part 3: Tree Type

To get more practice with pattern matching, custom data types and map/fold, let's build a `tree` data type!

First, we will define the `tree` type:

```ocaml
type 'a tree = 
  | Leaf 
  | Node of 'a tree * 'a * 'a tree
```

This recursively defines a `tree` to either be a
- `Leaf` 
- `Node` with a left sub-`tree`, a value, and a right sub-`tree`

Let's generalize `map` and `fold` to work on this `tree`. Try to implement it on your own! Can you describe the what the type of these functions should be?

To practice with this, let's write a few functions using map and fold define on trees:


- Write a function to return a `tree` with each value incremented by one:

- Write a function to return the sum of all the elements of a `tree`:

# Tail Recursion

## Tail Recursion

To recap, OCaml uses a **lot** of recursion. Recursive functions are just functions that call themselves; recall that recursive functions will accumulate recursive calls on a callstack. 


We can use **tail recursion** to optimize recursive functions. A tail recursive function is defined as a recursive function in which the recursive call is the last statement that is executed by the function. Tail recursive functions are more efficient than non-tail recursive functions.

In non-tail recursive functions, calculation will happen ***after*** the recursion call. In tail recursive functions, the calculation will occur ***before*** the recursive call, so it is more efficient. This is because we don't need to evaluate a bunch of recursive calls before we get the answer; the answer has already been computed by the time we want it!

We will go through some examples of recursive functions as well as tail-recursive implementations of those functions.

### Fold:

Non-tail recursive (`fold_right`)
```ocaml
let rec fold_right f xs a = match xs with
| [] -> a
| x :: xt -> f x (fold_right f xt a) ;;
```

Tail-recursive (`fold_left`)
```ocaml
let rec fold_left f a xs = match xs with
| [] -> a
| x :: xt -> fold f (f a x) xt ;;
```

### Factorial:

Non-tail recursive

```ocaml
let rec factorial num =
	if num > 1 then num * factorial (num - 1)
	else 1 ;;
```

Tail-recursive
```ocaml
let factorial num =
  if num = 0 then 1
  else if num = 0 then 1
  else
	let rec helper n acc =
	  if n > 0 then helper (n-1) (acc * n)
	  else acc
	in helper num 1 ;;
```

### Fibonacci:

Non-tail recursive
```ocaml
let rec fibonacci n = 
	if n = 1 then 1
	else if n = 2 then 1
	else fib (n-1) + fib (n-2) ;;
```

Tail-recursive
```ocaml
let fibonacci n = 
  if n = 1 then 1 
  else if n = 2 then 1 
  else 
	let rec helper n first second = 
	  if n > 2 then helper (n-1) second (first + second)
	  else second
	in helper n 1 1 ;;
```

Observe that when writing tail-recursive versions of `factorial` and `fibonacci`, that we define a helper function which stores the result of the calculation at each given step (similar to the accumulator in fold). More precisely, the helper function stores the **current result** and the number of the **current step**. Generally, this is the easiest way to convert a non-tail recursive function into a tail recursive function.

## Property Based Testing

Testing on particular examples is call __unit testing__ while testing on arbitrary inputs to validate the properties of output is called **property-based testing**.

Set up to write PBTs.

```
opam install qcheck 	# terminal
open QCheck				# in your .ml file
#require "qcheck"		# in utop before running open QCheck
(libraries qcheck)		# in dune file
```

### Anatomy of a test

```ocaml
let test_name =
  QCheck.Test.make 		# call the function to make the test
	~count:1000			# number of inputs to test the function on
    ~name:"reverse"			# name of the test in verbose
    QCheck.(list int)					# type of input
    (fun lst -> prop_reverse_reverses_the_list lst);;	# anonymous function that asserts the prop
```

### Example 1: Reversing a list

Given a function:

```ocaml
let reverse lst = List.fold_left (fun a x -> x :: a) [] lst;;
```

We want to test it using property based testing.

What are some properties of the function `reverse` and its output?

### Example 2: Deleting an element from the list

Given the delete function:

```ocaml
let delete lst elem = List.fold_right (fun x a -> if x = elem then a else x :: a) lst [];;
```
