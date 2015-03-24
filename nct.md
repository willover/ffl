### Module Description ###
The nct module implements a n-Tree that can store cell size data. It extends
the base n-Tree module, with cell based words. For adding and removing cells
to and from the tree, use the iterator [nci](nci.md).

### Module Words ###
#### Tree structure ####
**nct%** ( -- n )
> Get the required space for a nct variable
#### Tree creation, initialisation and destruction ####
**nct-init** ( nct -- )
> Initialise the tree
**nct-(free)** ( nct -- )
> Free the nodes in the tree
**nct-create** ( "`<`spaces`>`name" -- ; -- nct )
> Create a named n-tree in the dictionary
**nct-new** ( -- nct )
> Create a new n-tree on the heap
**nct-free** ( nct -- )
> Free the tree from the heap
#### Member words ####
**nct-length@** ( nct -- u )
> Get the number of nodes in the tree
**nct-empty?** ( nct -- flag )
> Check for empty tree
#### Tree words ####
**nct-clear** ( nct -- )
> Delete all nodes from the tree
**nct-execute** ( i\*x xt nct -- j\*x )
> Execute xt for every node in tree
**nct-execute?** ( i\*x xt nct -- j\*x flag )
> Execute xt for every node in the tree or until xt returns true, flag is true if xt returned true
**nct-count** ( x nct -- u )
> Count the number of the occurrences of the cell data x in the tree
**nct-has?** ( x nct -- flag )
> Check if the cell data x is present in the tree
#### Inspection ####
**nct-dump** ( nct -- )
> Dump the tree


---

Generated by **ofcfrth-0.10.0**