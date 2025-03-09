#### Create builtin logical operators *or* `||`, *and* `&&` and *not* `!` and add them to the language.

~~~c
symbol : /[a-zA-Z0-9_+\\-*\\/\\\\=<>!&|]+/ ;         \
lenv_add_builtin(e, "&&", builtin_and);
lenv_add_builtin(e, "||", builtin_or);
lenv_add_builtin(e, "!", builtin_not);
lval *builtin_and(lenv *e, lval *a) {
    return builtin_ord(e, a, "&&");
}

lval *builtin_or(lenv *e, lval *a) {
    return builtin_ord(e, a, "||");
}

lval *builtin_not(lenv *e, lval *a) {
    return builtin_ord(e, a, "!");
}
lval *builtin_ord(lenv *e, lval *a, char *op) {
    int r;
    if (strcmp(op, "!") == 0) {
        LASSERT_NUM(op, a, 1);
        r = !a->cell[0]->num;
        lval_del(a);
        return lval_num(r);
    }
    LASSERT_NUM(op, a, 2);
    LASSERT_TYPE(op, a, 0, LVAL_NUM);
    LASSERT_TYPE(op, a, 1, LVAL_NUM);
    if (strcmp(op, ">") == 0) {
        r = (a->cell[0]->num > a->cell[1]->num);
    }
    if (strcmp(op, "<") == 0) {
        r = (a->cell[0]->num < a->cell[1]->num);
    }
    if (strcmp(op, ">=") == 0) {
        r = (a->cell[0]->num >= a->cell[1]->num);
    }
    if (strcmp(op, "<=") == 0) {
        r = (a->cell[0]->num <= a->cell[1]->num);
    }
    if (strcmp(op, "&&") == 0) {
        r = (a->cell[0]->num && a->cell[1]->num);
    }
    if (strcmp(op, "||") == 0) {
        r = (a->cell[0]->num || a->cell[1]->num);
    }
    lval_del(a);
    return lval_num(r);
}
~~~

#### Define a recursive Lisp function that returns the `nth` item of that list.

~~~c
def {func} (\ {n list} {if (== n 1) {head list} {(func (- n 1) (tail list))}})
~~~

#### Define a recursive Lisp function that returns `1` if an element is a member of a list, otherwise `0`.

~~~c
def {in} (\ {x list} { if (== list {}) {0} { if (== (eval (head list)) x) {1} {in x (tail list)}}})
~~~

#### Define a Lisp function that returns the last element of a list.

~~~c
def {last} (\ {list} {if (== (tail list) {}) {head list} {last (tail list)}})
~~~

#### Define in Lisp logical operator functions such as `or`, `and` and `not`.

我们可以用``&``,``|``,``!!``来代替题一的``&&``,``||``,``!``。

#### Add a specific boolean type to the language with the builtin variables `true` and `false`.

详细可见src