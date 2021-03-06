# Project has been moved to [L8D/delvs](https://github.com/L8D/delvs) for naming and forking purposes.

Delvs
=====

Delvs(Pronounced Del-vis) is my own personal variation of brainfuck, the interpreter is written in C.
File reading was inspired by brainfuck++, and the bit fiddling was inspired by boolfuck(DuckDuckGo them if you have to, I'm not putting links there right now)

Additions
---------
- `:` casts the current value to a string and prints it.
- `;` grabs value from user input and casts to cell value.
- `#` reads following cells as null-terminating string for filename, then if current cell is != 1, open file for reading, or if 1, open file writing.
- `` ` `` read next char from file, apply to current cell.
- `!` write char from current cell to file.
- `"` increment current bit position by 1.
- `'` flip bit at current bit position, then incrment current bit position by 1.
- `\ ...` ignore EVERYTHING until newline is reached.
- `$` sleeps the current cell's value in seconds.
- `%` reads following cells as null-terminating string for address, and null-spaced port after that, then it opens a socket at that address.
- `^` sends current cell to socket.
- `&` reads on byte from socket and apply to current cell.
- `*` simply flushes stdout.

Corresponding lines of code:

- `:` `printf("%hhd", *p);`
- `;` `scanf("%hhd", p);`
- `#`  `f = fopen(p + 1, p == 1 ? "w" : "r");`
- `` ` `` `*p = fgetc(f);`
- `!` `fputc(*p, f);`
- `"` `b++;`
- `'` `*p ^= (1 << b)`
- `\ ...` `while(*c) if(*c++ == '\') return;`
- `$` `sleep(*p);`
- `%` `makesocket(p, (int)*++t);`
- `^` `send(s, p, 1, 0);`
- `&` `recv(s, p, 1, 0);`
- `*` `fflush(stdout);`

### To do
- implement runtime args
- implement namespaces(seperate arrays of 30000 chars)
- (maybe) implement better way to divide

Examples
--------
Setting entire cell values with just the bit intructions is as easy as converting 0's to `"`s and 1's to `'`s.

    01001000 = 72
    "'""'""" . >
    01101001 = 105
    "''"'""' .

Block comments are very straight forward to use, just plant a `\[` when you start "commenting" and a `]` when you're done.

    +++++ +++++ [
      > +++++ ++
      > +++++ +++++
      \ This is a well placed comment. See, all these code intrustions aren't messing with the program unlike vanilla Brainfuck
      > +++
      > +
      <<<< -
    ]
    > ++ .
    > + .
    +++++ ++ . .
    \ Another intruding comment.
    +++ .
    > ++ .
    << +++++ +++++ +++++ .
    > . +++ .
    ------ . -------- .
    > + .
    > .

How about reading files? Just generate a string as the filename an the use a `#` at the beginning of it. Like so:

    01101101 m
    "''"''"'
    01111001 y
    "''''""'
    01110100 t
    "'''"'""
    01100101 e
    "''""'"'
    01111000 x
    "''''"""
    01110100 t
    "'''"'""
    01100110 f
    "''""''"
    01101001 i
    "''"'""'
    01101100 l
    "''"''""
    01100101 e
    "''""'"'
    00101110\.
    ""'"'''"
    01110100 t
    "'''"'""
    01111000 x
    "''''"""
    01110100 t
    "'''"'""

    [<] #
    \ standard concatenation loop with file reading(and EOF check)
    `+[-.`+]

Want to give the actual value of the cell when printing? Just add a dot above your intruction!

    01100100 \ 'd' or 100
    "''""'""

    . \ prints "d"
    : \ prints "100"
