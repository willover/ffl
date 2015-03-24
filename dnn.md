### Module Description ###
The dnn module implements a node in a dnl list.

### Module Words ###
#### Node structure ####
**dnn%** ( -- n )
> Get the required space for a dnn structure
#### Node creation, initialisation and destruction ####
**dnn-init** ( dnn -- )
> Initialise the node
**dnn-new** ( -- dnn )
> Create a new node on the heap
**dnn-free** ( dnn -- )
> Free the node from the heap
#### Members words ####
**dnn-next@** ( dnn1 -- dnn2 )
> Get the next node dnn2 from node dnn1
**dnn-next!** ( dnn1 dnn2 -- )
> Set for node dnn2 the next node to dnn1
**dnn-prev@** ( dnn1 -- dnn2 )
> Get from node dnn1 the previous node
**dnn-prev!** ( dnn1 dnn2 -- )
> Set for node dnn2 the previous node to dnn1
#### Inspection ####
**dnn-dump** ( dnn -- )
> Dump the node


---

Generated by **ofcfrth-0.10.0**