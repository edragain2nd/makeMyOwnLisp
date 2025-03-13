#### Adapt the builtin function `join` to work on strings.

~~~c
lval* builtin_join(lenv* e, lval* a) {

  for (int i = 0; i < a->count; i++) {
    LASSERT_TWO_TYPE("join",a,i,LVAL_QEXPR,LVAL_STR);
  }
  lval* x;
  if (a->cell[0]->type==LVAL_STR&&a->cell[1]->type==LVAL_STR) {

    char* tmp=malloc(strlen(a->cell[0]->str)+strlen(a->cell[1]->str)+1);
    strcpy(tmp,a->cell[0]->str);
    strcat(tmp, a->cell[1]->str);
    x=lval_str(tmp);
  }
  else {
    x = lval_pop(a, 0);

    while (a->count) {
      lval* y = lval_pop(a, 0);
      x = lval_join(x, y);
    }
  }
  lval_del(a);
  return x;
}

~~~

#### Adapt the builtin function `head` to work on strings.

~~~c
lval* builtin_head(lenv* e, lval* a) {
  LASSERT_NUM("head", a, 1);
  LASSERT_TWO_TYPE("head",a,0,LVAL_QEXPR,LVAL_STR);
  LASSERT_NOT_EMPTY("head", a, 0);
 if (a->cell[0]->type==LVAL_STR) {
   char* tmp=malloc(2);
   tmp[0]=a->cell[0]->str[0];
   tmp[1]='\0';
   lval_del(a);
   return lval_str(tmp);
 }
    lval* v = lval_take(a, 0);
    while (v->count > 1) { lval_del(lval_pop(v, 1)); }
    return v;
}
~~~

#### Adapt the builtin function `tail` to work on strings.

~~~c
lval* builtin_tail(lenv* e, lval* a) {
  LASSERT_NUM("tail", a, 1);
  LASSERT_TWO_TYPE("tail", a, 0, LVAL_QEXPR,LVAL_STR);
  LASSERT_NOT_EMPTY("tail", a, 0);
  if (a->cell[0]->type==LVAL_STR){
    char* tmp=malloc(strlen(a->cell[0]->str));
    for (int i=0;i<strlen(a->cell[0]->str);++i) {
      tmp[i]=a->cell[0]->str[i+1];
    }
    tmp[strlen(a->cell[0]->str)-1]='\0';
    lval_del(a);
    return lval_str(tmp);
  }
  lval* v = lval_take(a, 0);
  lval_del(lval_pop(v, 0));
  return v;
}
~~~

#### Create a builtin function `read` that reads in and converts a string to a Q-expression.

~~~c
lval *builtin_read(lenv *e, lval *a) {
    LASSERT_NUM("read", a, 1);
    LASSERT_TYPE("read", a, 0, LVAL_STR);
    LASSERT_NOT_EMPTY("read", a, 0);
    lval *v = lval_qexpr();
    for (int i = 0; i < strlen(a->cell[0]->str); ++i) {
        char *tmp = malloc(2);
        tmp[0] = a->cell[0]->str[i];
        tmp[1] = '\0';
        lval_add(v, lval_sym(tmp));
    }
    lval_del(a);
    return v;
}
~~~

#### Create a builtin function `show` that can print the contents of strings as it is (unescaped).

~~~c
lval *lval_null() {
    lval *v = malloc(sizeof(lval));
    v->type = LVAL_NULL;
    return v;
};
lval *builtin_show(lenv *e, lval *a) {
    LASSERT_NUM("show", a, 1);
    LASSERT_TYPE("show", a, 0, LVAL_STR);

    lval_print_str(a->cell[0]);

    lval_del(a);
    return lval_null();
}
~~~

引入了``lval_null``的类型，返回一个占位，更多的改动看source code

#### Create a special value `ok` to return instead of empty expressions `()`.

~~~c
lval *lval_ok() {
    lval *v = malloc(sizeof(lval));
    v->type = LVAL_OK;
    return v;
}
~~~

然后将``return lval_sexpr()``改为``return lval_ok()``即可

#### Add functions to wrap all of C's file handling functions such as `fopen` and `fgets`.

~~~c
lval* lval_file(FILE* file) {
    lval* v = malloc(sizeof(lval));
    v->type = LVAL_FILE;
    v->file = file;
    return v;
}
lval* builtin_fopen(lenv* e, lval* a) {
    LASSERT_NUM("fopen", a, 2);
    LASSERT_TYPE("fopen", a, 0, LVAL_STR);
    LASSERT_TYPE("fopen", a, 1, LVAL_STR);
    char* filename = a->cell[0]->str;
    char* mode = a->cell[1]->str;
    FILE* file = fopen(filename, mode);
    return lval_file(file);
}

lval* builtin_fgets(lenv* e, lval* a) {
    LASSERT_NUM("fgets", a, 1);
    LASSERT_TYPE("fgets", a, 0, LVAL_FILE);
    char* buffer = malloc(sizeof(char) * 1024);
    lval* context = lval_str(buffer);
    free(buffer);
    return context;
}
~~~

