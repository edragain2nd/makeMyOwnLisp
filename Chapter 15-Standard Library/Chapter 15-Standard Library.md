#### Rewrite the `len` function using `foldl`.

~~~lisp
(fun {len l} {
  foldl (\ {acc _} {+ acc 1}) 0 l
})
~~~

#### Rewrite the `elem` function using `foldl`.

~~~lisp
(fun {elem x l} {
  foldl (\ {acc y} {or acc (== x y)}) false l
})
~~~

#### Incorporate your standard library directly into the language. Make it load at start-up.

pass

#### Write some documentation for your standard library, explaining what each of the functions do.

- **Atoms**:
  - `nil`: Represents an empty value.
  - `true`: Represents the boolean value `true` (1).
  - `false`: Represents the boolean value `false` (0).
- **Functional Functions**:
  - `fun`: Defines a function.
  - `let`: Opens a new scope.
  - `unpack`: Unpacks a list into a function call.
  - `pack`: Packs arguments into a list for a function call.
  - `curry`: Curries a function.
  - `uncurry`: Uncurries a function.
  - `do`: Executes a sequence of expressions.
- **Logical Functions**:
  - `not`: Logical NOT.
  - `or`: Logical OR.
  - `and`: Logical AND.
- **Numeric Functions**:
  - `min`: Returns the minimum of arguments.
  - `max`: Returns the maximum of arguments.
- **Conditional Functions**:
  - `select`: Selects the first true condition.
  - `case`: Matches a value against cases.
- **List Functions**:
  - `fst`: Returns the first element of a list.
  - `snd`: Returns the second element of a list.
  - `trd`: Returns the third element of a list.
  - `len`: Returns the length of a list.
  - `nth`: Returns the nth element of a list.
  - `last`: Returns the last element of a list.
  - `map`: Applies a function to each element of a list.
  - `filter`: Filters a list based on a predicate.
  - `init`: Returns all elements except the last.
  - `reverse`: Reverses a list.
  - `foldl`: Folds a list from the left.
  - `foldr`: Folds a list from the right.
  - `sum`: Sums the elements of a list.
  - `product`: Multiplies the elements of a list.
  - `take`: Takes the first `n` elements of a list.
  - `drop`: Drops the first `n` elements of a list.
  - `split`: Splits a list at position `n`.
  - `take-while`: Takes elements while a condition is true.
  - `drop-while`: Drops elements while a condition is true.
  - `elem`: Checks if an element exists in a list.
  - `lookup`: Looks up a key in a list of pairs.
  - `zip`: Zips two lists into a list of pairs.
  - `unzip`: Unzips a list of pairs into two lists.
- **Other Fun**:
  - `fib`: Computes the Fibonacci sequence.

#### Write some example programs using your standard library, for users to learn from.

~~~lisp
(sum (list 1 2 3 4 5)) ; Output: 15
(filter (\ {x} {== (% x 2) 0}) (list 1 2 3 4 5)) ; Output: (2 4)
(map fib (list 0 1 2 3 4 5)) ; Output: (0 1 1 2 3 5)
~~~

#### Write some test cases for each of the functions in your standard library.

~~~lisp
(assert (== (len (list 1 2 3)) 3))
(assert (== (len nil) 0))
(assert (elem 2 (list 1 2 3)))
(assert (not (elem 4 (list 1 2 3))))
(assert (== (sum (list 1 2 3)) 6))
(assert (== (sum nil) 0))
(assert (== (fib 0) 0))
(assert (== (fib 5) 5))
~~~

