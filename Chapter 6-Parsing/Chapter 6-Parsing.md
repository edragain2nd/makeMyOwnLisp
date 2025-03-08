#### Write a regular expression matching strings of all `a` or `b` such as `aababa` or `bbaa`.

~~~c
^[ab]+$
~~~

####  Write a regular expression matching strings of consecutive `a` and `b` such as `ababab` or `aba`.

~~~c
^(ab)+a?$
~~~

####  Write a regular expression matching `pit`, `pot` and `respite` but *not* `peat`, `spit`, or `part`.

~~~c
\b(pit|pot|respite)\b
~~~

####  Change the grammar to add a new operator such as `%`.

~~~c
operator : '+' | '-' | '*' | '/' | '%';                  \
~~~

####  Change the grammar to recognise operators written in textual format `add`, `sub`, `mul`, `div`.

~~~c
operator : \"add\" | \"sub\" | \"mul\" | \"div\";
~~~

####  Change the grammar to recognize decimal numbers such as `0.01`, `5.21`, or `10.2`.

~~~c
number   : /-?[0-9]+(\\.[0-9]+)?/ ;
~~~

#### Change the grammar to make the operators written conventionally, between two expressions.

~~~c
 mpca_lang(MPCA_LANG_DEFAULT,
      "                                                     \
      number   : /-?[0-9]+(\\.[0-9]+)?/ ;                             \
      operator : '+' | '-' | '*' | '/' | '%';                  \
      expr     : <number> | '(' <expr> <operator> <expr> ')' ;  \
      lispy    : /^/ <expr> /$/ ;             \
    ",
      Number, Operator, Expr, Lispy);
~~~

#### Use the grammar from the previous chapter to parse `Doge`. You must add *start* and *end* of input.

~~~c
mpca_lang(MPCA_LANG_DEFAULT,
  "                                                     \
  adjective : \"wow\" | \"many\" | \"so\" | \"such\" | \"very\"; \
  noun      : \"lisp\" | \"language\" | \"book\" | \"build\" | \"c\" | \"page\"; \
  phrase    : <adjective> <noun>;                       \
  doge      : /^/ <phrase>* /$/ ;                      \
  ",
  Adjective, Noun, Phrase, Doge);
~~~