### Module Description ###
The fst module implements a state in a Finite State Machine. See [fsm](fsm.md) for
more info about using this module.

### Module Words ###
#### State structure ####
**fst%** ( -- n )
> Get the required space for a state variable
#### State creation, initialisation and destruction ####
**fst-init** ( x xt1 xt2 c-addr u n fst -- )
> Initialise the state with id n and label c-addr u, entry action xt1, exit action xt2 and data x
**fst-(free)** ( fst -- )
> Free the internal, private variables from the heap
**fst-new** ( x xt1 xt2 c-addr u n -- fst )
> Create a new state on the heap with id n, label c-addr u, entry action xt1, exit action xt2 and data x
**fst-free** ( fst -- )
> Free the state from the heap
#### Member words ####
**fst-id@** ( fst -- n )
> Get the id of the state
**fst-label@** ( fst -- c-addr u )
> Get the label of the state
**fst-label?** ( c-addr u fst -- c-addr u false | fst true )
> Check the label c-addr u with this state
**fst-data@** ( fst -- x )
> Get the data of the state
**fst-data!** ( x fst -- )
> Set the data for the state
**fst-entry@** ( fst -- xt )
> Get the entry action of the state
**fst-exit@** ( fst -- xt )
> Get the exit action of the state
**fst-attributes!** ( c-addr u fst -- )
> Set the extra graphviz attributes for the state
**fst-attributes@** ( fst -- c-addr u )
> Get the extra graphviz attributes of the state
#### Transition words ####
**fst-new-transition** ( x xt c-addr u fst1 n fst -- ftr )
> Add a new transition to state fst1 with label c-addr u, number events n, action xt and data x
**fst-any-transition** ( x xt c-addr u fst1 fst -- ftr )
> Set the any transition to state fst1 with label c-addr u, action xt and data x
**fst-find-transition** ( c-addr u fst -- ftr | nil )
> Find the transition with label c-addr u, else return nil
#### Event words ####
**fst-feed** ( n fst -- fst | nil )
> Feed the event to this state, return the next state or nil if the event did not match any condition
**fst-try** ( n fst -- fst | nil )
> Try the event for this state, return the result
#### Inspection ####
**fst-dump** ( fst -- )
> Dump the fst variable
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