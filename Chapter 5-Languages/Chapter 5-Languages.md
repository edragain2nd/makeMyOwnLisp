#### Write down some more examples of strings the `Doge` language contains.

~~~c
 mpca_lang(MPCA_LANG_DEFAULT,
      "                                         \
      adjective : \"wow\" | \"many\"            \
                |  \"so\" | \"such\"| \" very\";           \
      noun      : \"lisp\" | \"language\"       \
                | \"book\" | \"build\" | \"c\"|\" page \"; \
      phrase    : <adjective> <noun>;           \
      doge      : <phrase>*;                    \
    ",
      Adjective, Noun, Phrase, Doge);
~~~

####  Why are there back slashes `\` in front of the quote marks `"` in the grammar?

为了在字符串里面合法地引用"号

####  Why are there back slashes `\` at the end of the line in the grammar?

确保字符串能跨越多行

#### Describe textually a grammar for decimal numbers such as `0.01` or `52.221`.

~~~c
" 												\
Number  : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'; \
Integer : <Number>+;							\
Faction : <Number>+;							\
Decimal : <Integer> '.' <Faction>;				\
"
~~~



####  Describe textually a grammar for web URLs such as `http://www.buildyourownlisp.com`.

~~~c
"                                               \
DNS : \"a-zA-Z0-9\"+;								\
url : \"https://www.\" <DNS> \".com\"";         \
"
~~~

这里省简便就不一一展开了

####  Describe textually a grammar for simple English sentences such as `the cat sat on the mat`.

~~~c
"												\
Preposition : \"on\" | \"in\";					\
Article  : \"the\"	| \"a\";					\
Noun     : \"cat\"  | \"dog\";					\
Verb     : \"jump\" | \"sat\";					\
PrepositionPhrase : <Preposition> <Article> <Noun> \
Subject  : <Article> <Noun>;					\		
Sentence : <Subject> <Verb> <PrepositionPhrase>;	\
"
~~~



####  Describe more formally the above grammars. Use `|`, `*`, or any symbols of your own invention.

前面的已经用到了，pass

####  If you are familiar with JSON, textually describe a grammar for it.

~~~c
"															\
JSON      : <Object> | <Array>;								\
Object    : '{' <Pair> (',' <Pair>)* '}' | \"{}\";			\
Pair      : <String> ':' <Value>;							\
Array     : '[' <Value> (',' <Value>)* ']' | \"[]\";		\
Value     : <String> | <Number> | <Object> | <Array> | \"true\" | \"false\" | \"null\";    \
String    : \"<Char>*\";									\
Char      : Any character except '"' and '\' | <EscapeSequence> \ 
EscapeSequence : '\' ('"' | '\' | '/' | 'b' | 'f' | 'n' | 'r' | 't' | 'u' HexDigit HexDigit HexDigit HexDigit);														\
HexDigit  : '0' | '1' | ... | '9' | 'a' | 'b' | ... | 'f' | 'A' | 'B' | ... | 'F'; \
Number    : <Integer> <Fraction>? <Exponent>?				\
Integer   : '-'? <Digit>+; 									\
Fraction  : '.' <Digit>+;									\
Exponent  : ('e' | 'E') ('+' | '-')? <Digit>+;				\
Digit     : '0' | '1' | ... | '9';							\
"
~~~

