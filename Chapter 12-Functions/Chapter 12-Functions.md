#### Define a Lisp function that returns the first element from a list.

~~~c
lispy> def {car} (\ {x} {head x})
()
lispy> car (list 1 2 3)
1
~~~

#### Define a Lisp function that returns the second element from a list.

~~~c
def {sec} (\ {x} {head (tail x)})
~~~

#### Define a Lisp function that calls a function with two arguments in reverse order.

~~~c
def {rev} (\ {x y z} {eval x z y})
~~~

#### Define a Lisp function that calls a function with arguments, then passes the result to another function.

~~~c
def {pass} (\ {callee caller arg1 arg2} {callee (caller arg1 arg2)})
~~~

#### Define a `builtin_fun` C function that is equivalent to the Lisp `fun` function.

~~~c
lval *builtin_fun(lenv *e, lval *a) {
    LASSERT_NUM("fun", a, 2);
    LASSERT_TYPE("fun", a, 0, LVAL_QEXPR);
    LASSERT_TYPE("fun", a, 1, LVAL_QEXPR);
    LASSERT_NOT_EMPTY("fun", a, 0);

    lval *func_name = lval_pop(a->cell[0], 0);
    func_name->type = LVAL_QEXPR;
    lval *lambda_func = lval_lambda(a->cell[0], a->cell[1]);
    lenv_def(e, func_name, lambda_func);
    lval_del(a);
    return lval_sexpr();
}
~~~

#### Change variable arguments so at least one extra argument must be supplied before it is evaluated.

在``lval_call``处理``x&xs``处加一个处理:

~~~c
LASSERT(a,a->count>=2,"at least one extra agrument!");
~~~

