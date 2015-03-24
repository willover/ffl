### Module Description ###
The fsm module implements a Finite State Machine. Use fsm-new-state to add
states to the machine. Then use fsm-new-transition to add transitions
between the states. fsm-new-transition returns the new transition. Use
ftr-condition@ on this new transition to get a reference to the condition
in the transition. This is actually a bit array, see [bar](bar.md). Use the words
of the bar module to set the condition. When the whole FSM is built, start
using the machine by calling fsm-start. By default the first created
state is the start state, but this can be changed by fsm-start! After
starting the machine, feed events to the machine by fsm-feed. This word
returns the new, current state or nil if no transition matched. The
machine can be converted to graphviz's dot files by fsm-to-dot. This word
uses the labels of the states and transitions to build the dot
representation. It also set the shape of the states (double circle for
start and end states, circles for the others). Use fst-attributes! and
ftr-attributes! to set additional graphviz attributes.
During the feeding of events, the optional actions are called. When a
state is left, the exit action is called, when a state is entered the
entry state is called. If a transition matched, the action of this
transition is also called. The stack usage for those actions:
```
state entry action:  fst -- = State fst is entered
state exit action:   fst -- = State fst is left
transition action: n ftr -- = Transition fst matched for event n
```

### Module Words ###
#### fsm structure ####
**fsm%** ( -- n )
> Get the required space for a fsm variable
#### FSM creation, initialisation and destruction ####
**fsm-init** ( +n fsm -- )
> Initialise the fsm with the number of events n
**fsm-(free)** ( fsm -- )
> Free the internal, private variables from the heap
**fsm-create** ( "`<`spaces`>`name" +n -- ; -- fsm )
> Create a named fsm in the dictionary with the number of events n
**fsm-new** ( +n -- fsm )
> Create a new fsm on the heap with the number of events n
**fsm-free** ( fsm -- )
> Free the fsm from the heap
#### Member words ####
**fsm-start@** ( fsm -- fst )
> Get the start state
**fsm-start!** ( fst fsm -- )
> Set the start state
#### State words ####
**fsm-new-state** ( x xt1 xt2 c-addr1 u1 fsm -- fst )
> Add a new state with label c-addr1 u1, entry action xt1, exit action xt2 and data x
**fsm-start** ( fsm -- )
> Start the finite state machine
**fsm-find-state** ( c-addr u fsm -- fst | nil )
> Find the state by its label c-addr u in the fsm
#### Transition words ####
**fsm-new-transition** ( x xt c-addr1 u1 fst1 fst2 fsm -- ftr )
> Add a new transition from state fst1 to state fst2 with label c-addr1 u1, action xt and data x
**fsm-any-transition** ( x xt c-addr1 u1 fst1 fst2 fsm -- ftr )
> Set the any transition for state fst1 to state fst2 with label c-addr1 u1, action xt and data x
#### Event words ####
**fsm-feed** ( n fsm -- fst | nil )
> Feed the event to the current state, return the next state or nil if the event did not match any condition
**fsm-try** ( n fsm -- fst | nil )
> Try the event for the current event, return the next state, but do not move to this state
#### Conversion words ####
**fsm-to-dot** ( c-addr u tos fsm -- )
> Convert the fsm to a dot string using the stream, giving the graph the name c-addr u
#### Inspection ####
**fsm-dump** ( fsm -- )
> Dump the fsm variable
### Examples ###
```
include ffl/fsm.fs
include ffl/enm.fs


\ Example: vending machine


\ Enumerate the events

begin-enumeration
enum: voilence
enum: coin
enum: choice
enum: refuse
enum: #events
end-enumeration


\ Create the finite state machine on the heap with #events events

#events fsm-new value machine


\ Create the entry and exit action words

: say-thank-you  ( fst -- = Say 'thank you' after a coin or choice )
  drop
  ." Thank you" cr
;

: say-choice?    ( fst -- = Ask for choice after the coin )
  drop
  ." Please make your choice" cr
;

: say-coin?      ( fst -- = Ask for coin after the choice )
  drop
  ." Please enter your coin" cr
;

: call-support   ( fst -- = Call support after voilence, states data contains the phone number )
  ." Voilence against the machine, calling: " fst-data@ . cr
;


\ Create the states in the state machine

0     nil            ' say-thank-you s" start"    machine fsm-new-state value start
0   ' say-choice?    ' say-thank-you s" choice?"  machine fsm-new-state value choice?
0   ' say-coin?      ' say-thank-you s" coin?"    machine fsm-new-state value coin?
112 ' call-support     nil           s" support"  machine fsm-new-state value support


\ Set extra dot attributes for the support state

s" color=red" support fst-attributes!


\ Create the transitions for state start, use the bit array to set the condition

0     nil             s" coin"      start choice? machine fsm-new-transition ftr-condition@ coin     swap bar-set-bit
0     nil             s" choice"    start coin?   machine fsm-new-transition ftr-condition@ choice   swap bar-set-bit
0     nil             s" voilence"  start support machine fsm-new-transition ftr-condition@ voilence swap bar-set-bit

s" voilence" start fst-find-transition s" color=yellow" rot ftr-attributes!


\ Create the transition actions for choice? and coin? states

: deliver-choice   ( n ftr -- = Deliver the choosen product )
  2drop
  ." Deliver choice" cr
;

: do-refund        ( n ftr -- = Refused the product, refund the coin )
  2drop
  ." Refund coin" cr
;


\ Create the transitions for state choice?

0   ' deliver-choice  s" choice"    choice? start   machine fsm-new-transition ftr-condition@ choice   swap bar-set-bit
0   ' do-refund       s" refuse"    choice? start   machine fsm-new-transition ftr-condition@ refuse   swap bar-set-bit
0     nil             s" voilence"  choice? support machine fsm-new-transition ftr-condition@ voilence swap bar-set-bit


\ Set extra dot attributes for the voilence transition

s" voilence" choice? fst-find-transition s" color=yellow" rot ftr-attributes!


\ Create the transitions for state coin?

0   ' deliver-choice  s" coin"      coin? start   machine fsm-new-transition ftr-condition@ coin     swap bar-set-bit
0     nil             s" refuse"    coin? start   machine fsm-new-transition ftr-condition@ refuse   swap bar-set-bit
0     nil             s" voilence"  coin? support machine fsm-new-transition ftr-condition@ voilence swap bar-set-bit

s" voilence" coin? fst-find-transition s" color=yellow" rot ftr-attributes!


\ Start the state machine and feed it events 

machine fsm-start

coin     machine fsm-feed drop
choice   machine fsm-feed drop
coin     machine fsm-feed drop
refuse   machine fsm-feed drop
choice   machine fsm-feed drop
coin     machine fsm-feed drop
voilence machine fsm-feed drop


\ Create a text output stream for writing the state machine to dot

tos-new value dot-tos


\ Create the writer word

: write-dot    ( c-addr u file-id -- flag = Write the string )
  write-line 
  0=
;


\ Create the output file

s" out.dot" w/o create-file throw value dot-file


\ Set the writer word and the file in the stream

dot-file  ' write-dot  dot-tos  tos-set-writer


\ Write the state machine to the dot-file with graph name "Machine"

s" Machine" dot-tos machine fsm-to-dot


\ Free the dot stream

dot-tos tos-free


\ Close the dot file

dot-file close-file throw


\ To generate an image, use for example: dot -Tpng -o fsm.png out.dot

\ ==============================================================================
```

---

Generated by **ofcfrth-0.10.0**