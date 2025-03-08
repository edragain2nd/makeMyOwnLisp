#### Run the previous chapter's code through `gdb` and crash it. See what happens.

~~~bash
lispy> / 10 0
Process finished with exit code 136 (interrupted by signal 8:SIGFPE)
~~~

####  How do you give an `enum` a name?

like this:

~~~c
enum Weekday {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};
~~~

#### What are `union` data types and how do they work?

union是在同一内存位置存储不同的数据类型。

~~~c
union Data {
    int i;
    float f;
    char str[20];
};
~~~

联合体的大小为20字节，为char str[20]的大小

#### What are the advantages over using a `union` instead of `struct`?

由于 `union` 的所有成员共享同一块内存空间，因此它可以节省内存												

#### Can you use a `union` in the definition of `lval`?

不行，我们没法判断1,0或者其他数字为type还是为number

#### Extend parsing and evaluation to support the remainder operator `%`.

~~~c
enum { LERR_DIV_ZERO, LERR_BAD_OP, LERR_BAD_NUM, LERR_REM_ZERO };
if (v.err == LERR_REM_ZERO) {
                printf("Error: Invalid Number!");
            }
if (strcmp(op, "%") == 0) {
        /* If second operand is zero return error */
        return y.num == 0
                   ? lval_err(LERR_REM_ZERO)
                   : lval_num(x.num % y.num);
    }
~~~

#### Extend parsing and evaluation to support decimal types using a `double` field.

修改一下paser和struct里num的类型，改一下printf的形式，详情看code