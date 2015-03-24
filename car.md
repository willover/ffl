### Module Description ###
The car module implements a dynamic cell array.

### Module Words ###
#### Cell Array Structure ####
**car%** ( -- n )
> Get the required space for a car variable
#### Array creation, initialisation and destruction ####
**car-init** ( +n car -- )
> Initialise the array with an initial length n
**car-(free)** ( car -- )
> Free the internal data from the heap
**car-create** ( n "`<`spaces`>`name" -- ; -- car )
> Create a cell array in the dictionary with an initial length n
**car-new** ( n -- car )
> Create a cell array with an initial length n on the heap
**car-free** ( car -- )
> Free the array from the heap
#### Member words ####
**car-length@** ( car -- u )
> Get the number of elements in the array
**car-index?** ( n car -- flag )
> Check if the index n is valid in the array
**car-size!** ( +n car -- )
> Insure the size of the array
**car-extra@** ( car -- u )
> Get the extra space allocated during resizing of the array
**car-extra!** ( u car -- )
> Set the extra space allocated during resizing of the array
**car+extra@** ( -- u )
> Get the initial extra space allocated during resizing of the array
**car+extra!** ( u -- )
> Set the initial extra space allocated during resizing of the array
**car-compare@** ( car -- xt )
> Get the compare execution token for sorting the array
**car-compare!** ( xt car -- )
> Set the compare execution token for sorting the array
#### Array words ####
**car-clear** ( car -- )
> Clear the array
**car-set** ( x n car -- )
> Set the cell value x at the nth element
**car-get** ( n car -- x )
> Get the cell value from the nth element
**car-append** ( x car -- )
> Append the cell value at the end of the array
**car-prepend** ( x car -- )
> Prepend the cell value at the start of the array
**car-insert** ( x n car -- )
> Insert the cell value x at the nth element
**car-delete** ( n car -- x )
> Delete the cell value at the nth position, return the previous cell value x
#### Sorting words ####
**car-sort** ( car -- )
> Sort the array using the compare execution token using heap sort
**car-find-sorted** ( x car -- n flag )
> Find the cell value x in the already sorted array using binary search, return offset and success
**car-has-sorted?** ( x car -- flag )
> Check if the cell value x is present in an already sorted array
**car-insert-sorted** ( x car -- )
> Insert the cell value x sorted in an already sorted array
#### Fifo/Lifo words ####
**car-tos** ( car -- x )
> Get the cell value at the end of the array, exception if array is empty
**car-push** ( x car -- )
> Push the cell value x at the end of the array
**car-pop** ( car -- x )
> Pop the cell value from the end of the array, exception if array is empty
**car-enqueue** ( x car -- )
> Enqueue the cell value x at the start of the array
**car-dequeue** ( car -- x )
> Dequeue the cell value from the end of the array, exception if array is empty
#### Special words ####
**car-count** ( x car -- u )
> Count the number of occurrences of a cell value x in the array
**car-find** ( x car -- n )
> Find the offset first occurrence of the cell value x in the array, -1 if not found
**car-has?** ( x car -- flag )
> Check if a cell value x is present in the array
**car-execute** ( i\*x xt car -- j\*x )
> Execute the execution token for every cell in the array
**car-execute?** ( i\*x xt car -- j\*x flag )
> Execute the execution token for every cell in the array or until xt returns true, flag is true if xt returned true
#### Inspection ####
**car-dump** ( car -- )
> Dump the cell array variable
### Examples ###
```
include ffl/car.fs


\ Example: simple numeric values in an array


\ Create a dynamic cell array in the dictionary with an initial size of 10

10 car-create values


\ Put values in the array

 7  0 values car-set                             \ Put value 7 on index 0 in values
 1  1 values car-set
 9  2 values car-set
 2  3 values car-set
 4  4 values car-set
 6  5 values car-set
 3  6 values car-set
 5  7 values car-set
 8  8 values car-set
 0 -1 values car-set                             \ Put value 0 on index 9 in values

10  values car-prepend                           \ Prepend value 10 at the start of the array
11  values car-append                            \ Append value 11 at the end of the array

12 5 values car-insert                           \ Insert value 12 on index 5 in the array


\ Get values from the array

.( Value on index 7: ) 7 values car-get . cr     \ Get the value on index 7 (6)

.( Delete second: ) 1 values car-delete . cr     \ Delete the value on index 1 and print this (7)

.( Length: ) values car-length@ . cr             \ Print the length of the array (12)

6 values car-has? [IF]                           \ Check the presence of a value in the array
  .( Value 6 is in the array.) cr
[ELSE]
  .( Value 6 is not in the array.) cr
[THEN]

9 values car-find dup -1 <> [IF]                 \ Find the index of a value in the array
  .( Value 9 is in the array on index: ) . cr
[ELSE]
  drop
  .( Value 9 is not in the array.) cr
[THEN]

values car-sort                                  \ Sort the values in the array

.( Values: ) ' . values car-execute cr           \ Print all values in the array



\ Example 2: store references to date/times in the array

include ffl/dtm.fs


\ Allocate a dynamic array with an initial size of 5 on the heap

5 car-new value dates


\ Create and add five dates to the array

dtm-new 0 dates car-set
dtm-new 1 dates car-set
dtm-new 2 dates car-set
dtm-new 3 dates car-set
dtm-new 4 dates car-set


\ Set the dates

23 dtm.february 2008  0 dates car-get  dtm-set-date   \ saturday
 3 dtm.march    2008  1 dates car-get  dtm-set-date   \ monday
24 dtm.january  2008  2 dates car-get  dtm-set-date   \ thursday
14 dtm.april    2008  3 dates car-get  dtm-set-date   \ monday
 4 dtm.may      2008  4 dates car-get  dtm-set-date   \ sunday

 
\ Sort the dates based on week day

: dates-compare  ( dtm1 dtm2 -- n = Compare two dates for week day )
  dtm-weekday swap dtm-weekday swap <=>
;

' dates-compare dates car-compare!                   \ Set the compare word for sorting

dates car-sort                                       \ Sort the array using the dates-compare word


\ Print the dates

: dates-print  ( dtm -- = Print the date )
  >r ." Day:" r@ dtm-day@ 2 .r ."  Month:" r@ dtm-month@ 2 .r ."  Year:" r> dtm-year@ . cr
;

' dates-print dates car-execute                      \ Print all the dates sorted on week day in the array


\ Free the dates from the heap

' dtm-free dates car-execute


\ Free the array from the heap

dates car-free

```

---

Generated by **ofcfrth-0.10.0**