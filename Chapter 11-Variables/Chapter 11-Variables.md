#### Create a Macro to aid specifically with reporting type errors.

本来想自己做,但是RTSC发现作者已经写好了

~~~c
#define LASSERT_TYPE(func, args, index, expect) \
  LASSERT(args, args->cell[index]->type == expect, \
    "Function '%s' passed incorrect type for argument %i. Got %s, Expected %s.", \
    func, index, ltype_name(args->cell[index]->type), ltype_name(expect))
~~~

#### Create a Macro to aid specifically with reporting argument count errors.

~~~c
#define LASSERT_NUM(func, args, num) \
  LASSERT(args, args->count == num, \
    "Function '%s' passed incorrect number of arguments. Got %i, Expected %i.", \
    func, args->count, num)
~~~

#### Create a Macro to aid specifically with reporting empty list errors.

~~~c
#define LASSERT_NOT_EMPTY(func, args, index) \
  LASSERT(args, args->cell[index]->count != 0, \
    "Function '%s' passed {} for argument %i.", func, index);
~~~

#### Change printing a builtin function so that it prints its name.

本来是打算引入一个func_name的变量，结果内存报错了，检查半天终于发现是忘写了copy lval部分.

~~~c
struct lval {
  int type;
  long num;
  char* err;
  char* sym;
  char* func_name;
  lbuiltin fun;
  int count;
  lval** cell;
};
lval* lval_fun(const char* name,lbuiltin func) {
  lval* v = malloc(sizeof(lval));
  v->type = LVAL_FUN;
  v->fun = func;
  v->func_name=malloc(strlen(name) + 1);
  strcpy(v->func_name,name);
  return v;
}
void lval_del(lval *v) {
    switch (v->type) {
        case LVAL_NUM: break;
        case LVAL_FUN: free(v->func_name);
            break;
        case LVAL_ERR: free(v->err);
            break;
        case LVAL_SYM: free(v->sym);
            break;
        case LVAL_QEXPR:
        case LVAL_SEXPR:
            for (int i = 0; i < v->count; i++) {
                lval_del(v->cell[i]);
            }
            free(v->cell);
            break;
    }

    free(v);
}
lval *lval_copy(lval *v) {
    lval *x = malloc(sizeof(lval));
    x->type = v->type;

    switch (v->type) {
        /* Copy Functions and Numbers Directly */
        case LVAL_FUN: x->fun = v->fun;
            x->func_name = malloc(strlen(v->func_name) + 1);
            strcpy(x->func_name, v->func_name);
            break;
        case LVAL_NUM: x->num = v->num;
            break;

        /* Copy Strings using malloc and strcpy */
        case LVAL_ERR:
            x->err = malloc(strlen(v->err) + 1);
            strcpy(x->err, v->err);
            break;

        case LVAL_SYM:
            x->sym = malloc(strlen(v->sym) + 1);
            strcpy(x->sym, v->sym);
            break;

        /* Copy Lists by copying each sub-expression */
        case LVAL_SEXPR:
        case LVAL_QEXPR:
            x->count = v->count;
            x->cell = malloc(sizeof(lval *) * x->count);
            for (int i = 0; i < x->count; i++) {
                x->cell[i] = lval_copy(v->cell[i]);
            }
            break;
    }

    return x;
}
void lval_print(lval* v) {
  switch (v->type) {
    case LVAL_FUN:   printf("%s", v->func_name); break;
    case LVAL_NUM:   printf("%li", v->num); break;
    case LVAL_ERR:   printf("Error: %s", v->err); break;
    case LVAL_SYM:   printf("%s", v->sym); break;
    case LVAL_SEXPR: lval_print_expr(v, '(', ')'); break;
    case LVAL_QEXPR: lval_print_expr(v, '{', '}'); break;
  }
}
void lenv_add_builtin(lenv* e, char* name, lbuiltin func) {
  lval* k = lval_sym(name);
  lval* v = lval_fun(name,func);
  lenv_put(e, k, v);
  lval_del(k); lval_del(v);
}
~~~

#### Write a function for printing out all the named values in an environment.

~~~c
void print_all_var(lenv* e) {
    for (int i=0;i<e->count;++i) {
        printf("%s=",e->syms[i]);
        lval_println(e->vals[i]);
    }
}
~~~

#### Redefine one of the builtin variables to something different.

~~~c
lval *builtin_add(lenv *e, lval *a) {
    return builtin_op(e, a, "-");
}
~~~

#### Change redefinition of one of the builtin variables to something different an error.

~~~c
lval *builtin_add(lenv *e, lval *a) {
    return lval_err("a error on propose");
}
lispy> + 1 2
Error: a error on propose
~~~

#### Create an `exit` function for stopping the prompt and exiting.

~~~c
lval *builtin_exit(lenv *e, lval *a)
{
    lval_del(a);
    exit(1);
}
~~~