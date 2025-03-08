#### What are the four typical steps for adding new language features?

| **Syntax**         | Add new rule to the language grammar for this feature.       |
| ------------------ | ------------------------------------------------------------ |
| **Representation** | Add new data type variation to represent this feature.       |
| **Parsing**        | Add new functions for reading this feature from the *abstract syntax tree*. |
| **Semantics**      | Add new functions for evaluating and manipulating this feature. |

#### Create a Macro specifically for testing for the incorrect number of arguments.

~~~c
#define LASSERT_NUM_ARGS(args, expected, err) \
  if ((args)->count != expected) { \
    lval_del(args); \
    return lval_err("Function passed incorrect number of arguments. Expected " #expected ", got %d.", (args)->count); \
  }
~~~

#### Create a Macro specifically for testing for being called with the empty list.

~~~c
#define LASSERT_NOT_EMPTY(args, err) \
  if ((args)->count == 0) { \
    lval_del(args); \
    return lval_err("Function passed empty list."); \
  }
~~~

#### Add a builtin function `cons` that takes a value and a Q-Expression and appends it to the front.

~~~c
lval *builtin_cons(lval *a) {
    LASSERT(a, a->count!=1, "Function 'cons' passed only one arguments.")
    LASSERT(a, a->count == 2,
            "Function 'cons' passed too many arguments.");
    LASSERT(a, a->cell[1]->type == LVAL_QEXPR,
            "Function 'cons' passed incorrect type.");
    a->type = LVAL_QEXPR;
    lval *x = lval_pop(a, 1);
    lval *result = lval_join(a, x);
    return result;
}
~~~

#### Add a builtin function `len` that returns the number of elements in a Q-Expression.

~~~c
lval *builtin_len(lval *a) {
    LASSERT(a, a->count == 1,
            "Function 'len' passed too many arguments.");
    LASSERT(a, a->cell[0]->type == LVAL_QEXPR,
            "Function 'len' passed incorrect type.");
    lval *x = lval_num(a->cell[0]->count);
    lval_del(a);
    return x;
}
~~~

#### Add a builtin function `init` that returns all of a Q-Expression except the final element.

~~~c
lval *builtin_init(lval *a) {
    LASSERT(a, a->count == 1,
            "Function 'init' passed too many arguments.");
    LASSERT(a, a->cell[0]->type == LVAL_QEXPR,
            "Function 'init' passed incorrect type.");
    lval *x = lval_pop(a->cell[0], a->cell[0]->count - 1);
    lval *y = lval_pop(a, 0);
    lval_del(x);
    lval_del(a);
    return y;
}
~~~