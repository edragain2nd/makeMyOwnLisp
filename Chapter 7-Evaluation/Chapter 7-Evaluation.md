####  Write a recursive function to compute the number of leaves of a tree.

~~~c
int number_of_leaves(mpc_ast_t *t) {
    if (t->children_num == 0) {
        return 1;
    }
    int total = 0;
    for (int i = 0; i < t->children_num; i++) {
        total += number_of_leaves(t->children[i]);
    }
    return total;
}
~~~

#### Write a recursive function to compute the number of branches of a tree.

~~~c
int number_of_branches(mpc_ast_t *t) {
    if (t->children_num == 0) {
        return 0;
    }
    int total = 1;
    for (int i = 0; i < t->children_num; i++) {
        total += number_of_branches(t->children[i]);
    }
    return total;
}
~~~

#### Write a recursive function to compute the most number of children spanning from one branch of a tree.

~~~c
int max_branch_node(mpc_ast_t *t) {
    if (t->children_num == 0)
        return 0;

    int max_number = t->children_num;

    for (int i = 0; i < t->children_num; i++) {
        int max_children = max_branch_node(t->children[i]);
        if (max_number < max_children)
            max_number = max_children;
    }
    return max_number;
}
~~~

#### How would you use `strstr` to see if a node was tagged as an `expr`?

它可以在一个字符串中查找子字符串

~~~c
strstr(t->tag, "expr")
~~~

#### How would you use `strcmp` to see if a node had the contents `'('` or `')'`?

~~~c
strcmp(t->contents, "(") == 0
strcmp(t->contents, ")") == 0
~~~

#### Add the operator `%`, which returns the remainder of division. For example `% 10 6` is `4`.

~~~c
if (strcmp(op, "%") == 0) { return x % y; }
~~~

#### Add the operator `^`, which raises one number to another. For example `^ 4 2` is `16`.

~~~c
if (strcmp(op, "^") == 0) {
        int cnt = 1;
        for (int i = 1; i <= y; i++) {
            cnt *= x;
        }
        return cnt;
    }
~~~

#### Add the function `min`, which returns the smallest number. For example `min 1 5 3` is `1`.

~~~c
if (strcmp(op, "min") == 0) {
        if (x < y)return x;
        return y;
    }
~~~

####  Add the function `max`, which returns the biggest number. For example `max 1 5 3` is `5`.

~~~c
if (strcmp(op, "max") == 0) {
        if (x > y)return x;
        return y;
    }
~~~

#### Change the minus operator `-` so that when it receives one argument it negates it.

~~~c
if (strcmp(op, "-") == 0 && t->children_num == 4) {
        return -eval(t->children[2]);
    }
~~~

