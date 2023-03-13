Introduction to OCaml

## Introduction

This exercise consists of a few short functions to help you familiarize yourself with OCaml.  You can review the content of the discussion in [notes.rb](notes.rb), which includes the topics and examples from the discussion video.  Also, check out [utop-tutorial.md](utop-tutorial.md) for an introduction to using `utop`.

### Testing & Submitting

You will submit this project to [Gradescope](https://www.gradescope.com/courses/171498/assignments/681774).  You may only submit the **disc3.rb** file.  To test locally, run `dune runtest -f`.

## Part 1: Type inference

At the top of [disc3.ml](src/disc3.ml), there are a few function definitions. Try determining the types of these functions and check your answers with `utop`.  This portion of the discussion is not tested (and thus not graded), but these kinds of exercises may appear on quizzes and exams!

## Part 2: Type definitions

You will have to fill in definitions for the functions `tf1`, `tf2`, `tf3` such that they have the type that is expected in the `.mli`. The operation of the function does not matter, as long as they have the correct types.

#### `tf1 a`

- **Type**: `string -> int`

#### `tf2 a b c`

- **Type**: `'a -> 'b -> 'b -> bool`

#### `tf3 a b`

- **Type**: `'a list -> 'a list -> 'a`
- **Note**: For this one, you can assume that the lists `a` and `b` are not empty.

## Part 3: Functions

#### `concat str1 str2`

- **Type**: `string -> string -> string`
- **Description**: Appends `str2` to the end of `str1`.
- **Examples**:
  ```ocaml
  concat "" "" = ""
  concat "" "abc" = "abc"
  concat "xyz" "" = "xyz"
  concat "abc" "xyz" = "abcxyz"
  ```

#### `add_to_float integer flt`

- **Type**: `int -> float -> float`
- **Description**: Adds `integer` and `flt` and returns a float representation of the sum.
- **Examples**:
  ```ocaml
  add_to_float 3 4.8 = 7.8
  add_to_float 0 0.0 = 0.0
  ```

#### `fib n`

- **Type**: `int -> int`
- **Description**: Calculates the nth [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number).
- **Examples**:
  ```ocaml
  fib 0 = 0
  fib 1 = 1
  fib 2 = 1
  fib 3 = 2
  fib 6 = 8
  ```

## Part 4: Lists

#### `add_three lst`

- **Type**: `int list -> int list`
- **Description**: Adds 3 to each element in `lst`.
- **Examples**:
  ```ocaml
  add_three [] = []
  add_three [1] = [4]
  add_three [1; 3; 5] = [4; 6; 8]
  ```

#### `filter n lst`

- **Type**: ``a -> `a list -> `a list`
- **Description**: Given `n` and a list `lst`, remove elements from `lst` that are greater than `n`.
- **Examples**:
  ```ocaml
  filter 2 [1; 2; 3; 3; 2; 1] = [1; 2; 2; 1]
  filter 5 [-1; 2; 3; 4] = [-1; 2; 3; 4]
  ```

#### `double lst`

- **Type**: `'a list -> 'a list`
- **Description**: Given a list `lst`, return a new list that has two copies of every element in `lst`.
- **Examples**: 
  ```ocaml
  double [1;2;3;4] = [1;1;2;2;3;3;4;4]
  double ["a"; "b"; "c"] = ["a"; "a"; "b"; "b"; "c"; "c"]
  ```
Datatypes & Higher Order Functions

## Introduction

This exercise consists of a couple of functions to get you familiarized with tuples and records, but mostly on higher order functions.  You can review the content of the discussion in [notes.rb](notes.rb), which includes the topics and examples from the discussion video.

### Testing & Submitting

You will submit this project to [Gradescope](https://www.gradescope.com/courses/171498/assignments/694478).  You may only submit the **disc4.ml** file.  To test locally, run `dune runtest -f`.

### Implementation

#### IMPORTANT NOTE 
You may **not** add the `rec` keyword to any of the functions you implement this week, and you may **not** implement any recursive helper functions. By following the video and notes sheet, you should be able to write these functions using only map, foldl, and foldr. 

#### `mul_thresh`

- **Type**: `int list -> int -> int * int`
- **Description**: Given a list of ints and another int `thresh`, return a tuple whose first element is the product of all elements in the list *less* than `thresh`, and whose second element is the product of all elements in the list *greater than or equal to* `thresh`. In the event that the list is empty, return the tuple (1,1), since this is the multiplicative identity, $a^0$
- **Examples**:
  ```ocaml
  mul_thresh [1;3;5;7] 6 = (15, 7)
  mul_thresh [] 6 = (1, 1) 
  mul_thresh [6;7] 6 = (1, 42)
  ```

#### `multi_map`

- **Type**: `('a-> 'b) -> 'a list list -> 'b list list`
- **Description**: Given a function `f` and a 2d list (a list of lists), return a new list in which each of the elements of the original list have had the function applied to them. 
As the name suggests, you might want to implement this function using multiple maps. 
- **Examples**:
  ```ocaml
  multi_map (fun x -> x ^ " is cool") [["shilpa"; "minya"];["vinnie";"pavan"]] = [["shilpa is cool"; "minya is cool"]; ["vinnie is cool"; "pavan is cool"]]
  multi_map float_of_int [[1;3;5];[2;4;6]] = [[1.;3.;5.];[2.;4.;6.;]] 
  multi_map float_of_int [[];[]] = [[];[]]
  ```

#### `update_database`

- **Type**: `val update_database : (string * int * float) list -> student_information list` 
- **Description**: Given a list of tuples of the following form: `(name, age, gpa)`, transform this list of tuples into a list of type `student_information`.  
- **Examples**: 
    ```ocaml 
    update_database [("alice", 21, 4.);("bob", 20, 3.85);("jess", 22, 2.9)] = [{name = "alice"; age = 21; gpa = 4.}; {name = "bob"; age = 20; gpa = 3.85}; {name = "jess"; age = 22; gpa = 2.9}]  
    update_database [] = []
    ``` 

#### `stalin_sort`

- **Type**: `'a list -> 'a list` 
- **Description**: Given a list, sort it using Stalin Sort, in which every element that is out of line is simply eliminated from the list. You **must** start from the left for this function.   
- **Examples**: 
    ```ocaml 
    stalin_sort [] = [] 
    stalin_sort [1;0;2;1;4;4] = [1;2;4;4] 
    stalin_sort [9;1;2;3;4;5] = [9]
    stalin_sort ["330";"is";"horrendous";"terrific"] = ["330"; "is"; "terrific"]
    ``` 

#### `stalin_sort_right`

- **Type**: `'a list -> 'a list` 
- **Description**: Given a list, sort it using Stalin Sort, in which every element that is out of line is simply eliminated from the list. You **must** start from the *right* for this function.    
- **Examples**: 
    ```ocaml 
    stalin_sort_right [] = [] 
    stalin_sort_right [1;0;2;1;4;4] = [0;1;4;4] 
    stalin_sort_right [9;1;2;3;4;5] = [1; 2; 3; 4; 5]
    stalin_sort_right ["216";"is";"terrific"; "lame"] = ["216"; "is"; "lame"]
    ``` 
- **Note**: 216 is not lame, it is an excellent course. It just isn't as good as 330. 
