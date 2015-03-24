### Module Description ###
The nfs module implements a state in a non-deterministic finite automata.

### Module Words ###
#### State structure ####
**nfs%** ( -- n )
> Get the required space for a nfs state
#### State types ####
**nfs.char** ( -- n )
> State type char, data = char
**nfs.any** ( -- n )
> State type any, no data
**nfs.class** ( -- n )
> State type class, data = chs
**nfs.lparen** ( -- n )
> State type left paren, data = paren level
**nfs.rparen** ( -- n )
> State type right paren, data = paren level
**nfs.split** ( -- n )
> State type split, no data
**nfs.match** ( -- n )
> State type match, no data
#### State creation, initialisation and destruction ####
**nfs-init** ( x n1 n2 nfs -- )
> Initialise the nfs state with data x, type n1 and id n2
**nfs-new** ( x n1 n2 -- nfs )
> Create a new nfs state on the heap with data x, type n1 and id n2
**nfs-free** ( nfs -- )
> Free the state from the heap
#### Member words ####
**nfs-id@** ( nfs -- n )
> Get the id of the state
**nfs-type@** ( nfs -- n )
> Get the type of the state
**nfs-data@** ( nfs -- x )
> Get the optional data of the state
**nfs-out1!** ( nfs1 nfs2 -- )
> Set out1 in the nfs2 state to the nfs1 state
**nfs-out1@** ( nfs1 -- nfs2 )
> Get the out1 state of the nfs1 state
**nfs-out2!** ( nfs1 nfs2 -- )
> Set out2 in the nfs2 state to the nfs1 state
**nfs-out2@** ( nfs1 -- nfs2 )
> Get the out2 nfs state of nfs1 state
**nfs-visit!** ( n nfs -- )
> Set the visit number [0`>`=]
**nfs-visit@** ( nfs -- n )
> Get the visit number
#### Inspection ####
**nfs-dump** ( nfs -- )
> Dump the nfs state


---

Generated by **ofcfrth-0.10.0**