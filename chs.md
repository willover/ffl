### Module Description ###
The chs module implements a character set. It supports the POSIX classes
used in regular expressions.

### Module Words ###
#### Character Set Structure ####
**chs%** ( -- n )
> Get the required space for a chs variable
#### Set creation, initialisation and destruction ####
**chs-init** ( chs -- )
> Initialise the set
**chs-create** ( "`<`spaces`>`name" -- ; -- chs )
> Create a named character set in the dictionary
**chs-new** ( -- chs )
> Create a new character set on the heap
**chs-free** ( chs -- )
> Free the set from the heap
#### Sets words ####
**chs^move** ( chs1 chs2 -- )
> Move chs1 in chs2
**chs^or** ( chs1 chs2 -- )
> OR the sets chs1 with chs2 and store the result in chs2
**chs^and** ( chs1 chs2 -- )
> AND the sets chs1 with chs2 and store the result in chs2
**chs^xor** ( chs1 chs2 -- )
> XOR the sets chs1 with chs2 and store the result in chs2
#### Set words ####
**chs-set** ( chs -- )
> Set all characters in the set
**chs-reset** ( chs -- )
> Reset all characters in the set
**chs-invert** ( chs -- )
> Invert all characters in the set
#### Char words ####
**chs-set-char** ( char chs -- )
> Set the character in the set
**chs-reset-char** ( char chs -- )
> Reset the character in the set
#### Character range words ####
**chs-set-chars** ( char1 char2 chs -- )
> Set the character range [char2..char1] in the set
**chs-reset-chars** ( char1 char2 chs -- )
> Reset the character range [char2..char1] in the set
#### String words ####
**chs-set-string** ( c-addr u chs -- )
> Set the characters in the string in the set
**chs-reset-string** ( c-addr u chs -- )
> Reset the characters in the string in the set
#### List words ####
**chs-set-list** ( charu .. char1 u chs -- )
> Set the characters char1 till charu in the set
**chs-reset-list** ( charu .. char1 u chs -- )
> Reset the characters char1 till charu in the set
#### POSIX classes ####
**chs-set-upper** ( chs -- )
> Set the upper class in the set
**chs-reset-upper** ( chs -- )
> Reset the upper class in the set
**chs-set-lower** ( chs -- )
> Set the lower  class in the set
**chs-reset-lower** ( chs -- )
> Reset the lower class in the set
**chs-set-alpha** ( chs -- )
> Set the alpha class in the set
**chs-reset-alpha** ( chs -- )
> Reset the alpha class in the set
**chs-set-digit** ( chs -- )
> Set the digit class in the set
**chs-reset-digit** ( chs -- )
> Reset the digit class in the set
**chs-set-alnum** ( chs -- )
> Set the alnum class in the set
**chs-reset-alnum** ( chs -- )
> Reset the alnum class in the set
**chs-set-xdigit** ( chs -- )
> Set the xdigit class in the set
**chs-reset-xdigit** ( chs -- )
> Reset the xdigit class in the set
**chs-set-punct** ( chs -- )
> Set the punct class in the set
**chs-reset-punct** ( chs -- )
> Reset the punct class in the set
**chs-set-blank** ( chs -- )
> Set the blank class in the set
**chs-reset-blank** ( chs -- )
> Reset the blank class in the set
**chs-set-space** ( chs -- )
> Set the space class in the set
**chs-reset-space** ( chs -- )
> Reset the space class in the set
**chs-set-cntrl** ( chs -- )
> Set the cntrl class in the set
**chs-reset-cntrl** ( chs -- )
> Reset the cntrl class in the set
**chs-set-graph** ( chs -- )
> Set the graph class in the set
**chs-reset-graph** ( chs -- )
> Reset the graph class in the set
**chs-set-print** ( chs -- )
> Set the print class in the set
**chs-reset-print** ( chs -- )
> Reset the print class in the set
**chs-set-word** ( chs -- )
> Set the word class in the set
**chs-reset-word** ( chs -- )
> Reset the word class in the set
#### Char check word ####
**chs-char?** ( char chs -- flag )
> Check if the character is in the set
#### Special words ####
**chs-execute** ( i\*x xt chs -- j\*x )
> Execute xt for every character in the set
**chs-execute?** ( i\*x xt chs -- j\*x flag )
> Execute xt for every character in the set or until xt returns true, flag is true if xt returned true
#### Inspection ####
**chs-dump** ( chs -- )
> Dump the chs state
### Examples ###
```
include ffl/chs.fs


\ Create a character set variable chs1 in the dictionary

chs-create chs1

\ Set 'a', '*', 'q', 'w' and '0'..'9' in the set

char a chs1 chs-set-char
char * chs1 chs-set-char

char 9 char 0 chs1 chs-set-chars

s" qw" chs1 chs-set-string

\ Check for characters in the set

char 7 chs1 chs-char? [IF]
  .( Character '7' is in the set ) cr
[ELSE]
  .( Character '7' is not in the set ) cr
[THEN]

char ; chs1 chs-char? [IF]
  .( Character ';' is in the set ) cr
[ELSE]
  .( Character ';' is not in the set ) cr
[THEN]

\ Create a character set on the heap

chs-new value chs2

\ Use the space and xdigit classes to fill the set 

chs1 chs-set-space
chs1 chs-set-xdigit

\ Invert the set so that the set contains no spaces and xdigits

chs1 chs-invert

\ Print the set contents by excecuting chs-emit for every character in the set

: chs-emit  ( char -- )
  dup chr-print? IF
    emit
  ELSE
    [char] < emit 0 .r [char] > emit
  THEN
;

.( Set:) ' chs-emit chs1 chs-execute cr

\ Free the set from the heap

chs2 chs-free

```

---

Generated by **ofcfrth-0.10.0**