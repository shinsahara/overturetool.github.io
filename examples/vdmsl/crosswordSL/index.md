---
layout: default
title: crossword
---

~~~

This tutorial example is taken out of a VDM course given to the students 

#******************************************************
~~~
###crossword.vdmsl

{% raw %}
~~~

values size : nat = 8;
types  word = seq of char 
        pos = nat1 
	grid = map position to char
	HV = <H> | <V>
state  crosswords of
functions
CW_INVARIANT: grid * set of word * set of word +> bool
WORDS : grid +> set of word
HOR_WORDS : grid +> set of word
LINE : pos *  grid +> seq of char
WORDS_OF_SEQ : seq of char +> set of word
COMPATIBLE : grid * word * position * HV +> bool
IS_LOCATED : grid * word * position * HV +> bool
IN_WORD: grid * position * HV +> bool

operations
VALIDATE_WORD (w : word)
ADD_WORD (w : word, p : position, d : HV)
ADD_BLACK ( p : position)
DELETE_BLACK ( p : position)
STRONG_DELETE (w : word, p : position, d : HV)
SOFT_DELETE (w : word, p : position, d : HV)

~~~{% endraw %}
