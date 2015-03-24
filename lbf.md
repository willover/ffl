### Module Description ###
The lbf module implements a linear buffer with variable elements. During
adding of extra data, the buffer will be resized. This type of buffer is
most efficient if the buffer is empty on a regular bases: the unused space
in the buffer is then automatically reduced. If the buffer is not
regularly empty, use the lbf-reduce word to reuse the unused space in the
buffer. The lbf-access! word expects two execution tokens on the stack:
store with stack effect:  i\*x addr --  and fetch: addr -- i\*x. Those two
words are used to store data in the buffer and fetch data from the buffer.
Their behavior should match the size of the elements in the buffer.
Besides the normal out pointer there is a secondary out pointer. This
pointer will always stay between the normal out pointer and the in
pointer. The words lbf-get' and lbf-length' use the secondary out pointer.
Important: the lbf-get and lbf-fetch returning addresses are located
in the buffer so the contents of these addresses can change with the next
call to the buffer. This is different from the circular buffer [cbf](cbf.md)
implementation: the cbf-get and cbf-fetch words copy data from the buffer
to the destination addresses.

### Module Words ###
#### Linear Buffer Structure ####
**lbf%** ( -- n )
> Get the required space for a lbf variable
#### Buffer creation, initialisation and destruction ####
**lbf-init** ( +n1 +n2 lbf -- )
> Initialise the buffer with element size n1 and initial length n2
**lbf-(free)** ( lbf -- )
> Free the internal data from the heap
**lbf-create** ( +n1 +n2 "`<`spaces`>`name" -- ; -- lbf )
> Create a linear buffer in the dictionary with element size n1 and initial length n2
**lbf-new** ( +n1 +n2 -- lbf )
> Create a linear buffer with element size n1 and initial length n2 on the heap
**lbf-free** ( lbf -- )
> Free the linear buffer from the heap
#### Member words ####
**lbf-length@** ( lbf -- u )
> Get the number of elements in the buffer
**lbf-length'@** ( lbf -- u )
> Get the number of elements in the buffer based on the secondary out pointer
**lbf-gap@** ( lbf -- u )
> Get the number of elements between the out pointer and the secondary out pointer
**lbf-extra@** ( lbf -- u )
> Get the extra space allocated during resizing of the buffer
**lbf-extra!** ( u lbf -- )
> Set the extra space allocated during resizing of the buffer
**lbf-size!** ( +n lbf -- )
> Insure the size of the buffer
**lbf+extra@** ( -- u )
> Get the initial extra space allocated during resizing of the buffer
**lbf+extra!** ( u -- )
> Set the initial extra space allocated during resizing of the buffer
**lbf-access@** ( lbf -- xt1 xt2 )
> Get the store word xt1 and the fetch word xt2 for the buffer
**lbf-access!** ( xt1 xt2 lbf -- )
> Set the store word xt1 and the fetch word x2 for the buffer
#### Lifo words ####
**lbf-set** ( addr u lbf -- )
> Set u elements, starting from addr in the buffer, resize if necessary
**lbf-get** ( u1 lbf -- addr u2 | 0 )
> Get maximum u1 elements from the buffer, return the actual number of elements u2
**lbf-get'** ( u1 lbf -- addr u2 | 0 )
> Get maximum u1 elements from the buffer, based on secondary out, return the actual number of elements u2
**lbf-fetch** ( u1 lbf -- addr u2 | 0 )
> Fetch maximum u1 elements from the buffer, return the actual number of elements u2
**lbf-skip** ( u1 lbf -- u2 )
> Skip maximum u1 elements from the buffer, return the actual skipped elements u2
**lbf-enqueue** ( i\*x lbf | addr lbf -- )
> Enqueue one element in the buffer, using the store word if available
**lbf-dequeue** ( lbf -- i\*x true | addr true | false )
> Dequeue one element from the buffer, using the fetch word if available
#### Fifo words ####
**lbf-tos** ( lbf -- i\*x true | addr true | false )
> Fetch the top element, using the fetch word if available
**lbf-push** ( i\*x lbf | addr lbf -- )
> Push one element in the buffer, using the store word if available
**lbf-pop** ( lbf -- i\*x true | addr true | false )
> Pop one element from the buffer, using the fetch word if available
#### Copy words ####
**lbf-copy** ( u1 u2 lbf -- )
> Copy records u1 times from distance u2, u1 `>`= u2 is allowed
#### Buffer words ####
**lbf-clear** ( lbf -- )
> Clear the buffer
**lbf-reduce** ( u lbf -- )
> Remove the leading unused space in the buffer if the unused length is at least u elements
#### Inspection ####
**lbf-dump** ( lbf -- )
> Dump the linear buffer variable
### Examples ###
```
include ffl/lbf.fs


\ Example 1: buffering characters strings


\ Create the lineair buffer in the dictionary with an initial size of 10 chars

1 chars 10 lbf-create char-buf


\ Put characters in the buffer

s" Hello" char-buf lbf-set

\ Get the length of the stored characters

.( Number characters in buffer:) char-buf lbf-length@ . cr

\ Put more characters in the buffer, resulting in a resize of the buffer

s" , a nice morning to you." char-buf lbf-set


\ Get characters from the buffer

.( Read the buffer:) 29 char-buf lbf-get type cr



\ Example 2: buffering compound data: pair of cells as element


\ Create the lineair buffer on the heap with an initial size of 3 elements

2 cells 3 lbf-new value xy-buf


\ Set the store and fetch words for the buffer

' 2! ' 2@ xy-buf lbf-access!


\ Use the buffer as fifo buffer, using the store and fetch words

1 2 xy-buf lbf-enqueue
3 4 xy-buf lbf-enqueue
5 6 xy-buf lbf-enqueue
7 8 xy-buf lbf-enqueue       \ Buffer is resized

\ Get the length of the stored elements in the buffer

.( Number elements in buffer:) xy-buf lbf-length@ . cr

\ Get first element from buffer

.( First pair in buffer:) xy-buf lbf-dequeue [IF]
  .  . cr
[ELSE]
  .(  nothing in buffer) cr
[THEN]


\ Use the buffer as lifo buffer, using the store and fetch words

\ Get last pair from buffer

.( Last pair in buffer:) xy-buf lbf-pop [IF]
  . . cr
[ELSE]
  .(  nothing in buffer) cr
[THEN]

\ Free the buffer from the heap

xy-buf lbf-free

\ ==============================================================================
```

---

Generated by **ofcfrth-0.10.0**