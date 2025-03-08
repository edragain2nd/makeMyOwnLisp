#### Give an example of a variable in our program that lives on *The Stack*.

~~~c
lval* lval_read_num(mpc_ast_t* t) {
  errno = 0;
  long x = strtol(t->contents, NULL, 10);
  return errno != ERANGE ?
    lval_num(x) : lval_err("invalid number");
}
~~~

这里的``errno``就是一个分配在``stack``的变量，随着函数结束，它的生命周期也结束。

#### Give an example of a variable in our program that points to *The Heap*.

```c
void lval_expr_print(lval* v, char open, char close) {
  putchar(open);
  for (int i = 0; i < v->count; i++) {

    /* Print Value contained within */
    lval_print(v->cell[i]);

    /* Don't print trailing space if last element */
    if (i != (v->count-1)) {
      putchar(' ');
    }
  }
  putchar(close);
}
```

这里面的``v``就是分配在``heap``上的，它的生命周期在函数结束后还持续，必须手动释放内存。

#### What does the `strcpy` function do?

将一个字符串从源地址复制到目标地址。

#### What does the `realloc` function do?

调整之前分配的内存块的大小。

#### What does the `memmove` function do?

将一块内存从源地址复制到目标地址。

#### How does `memmove` differ from `memcpy`?

`memcpy` 不处理内存区域重叠。如果源和目标区域重叠，为ub。

`memmove` 安全地处理重叠区域，防止数据损坏。

`memcpy` 更快，因为它假设没有重叠，不执行额外检查。

`memmove` 稍慢，因为需要检查重叠区域

#### Extend parsing and evaluation to support the remainder operator `%`.

和之前的工作相同，在相应位置补全即可，后面的题目和这个详情看code。

#### Extend parsing and evaluation to support decimal types using a `double` field.