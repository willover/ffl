### Module Description ###
The ftr module implements a transition in a Finite State Machine. See [fsm](fsm.md)
for more info about using this module.

### Module Words ###
#### ftr structure ####
**ftr%** ( -- n )
> Get the required space for a transition variable
#### Transition creation, initialisation and destruction ####
**ftr-init** ( x xt c-addr u fst +n ftr -- )
> Initialise the transition to the state fst, with label c-addr u, number events n, action xt and data x
**ftr-(free)** ( ftr -- )
> Free the internal, private variables from the heap
**ftr-new** ( x xt c-addr u fst +n -- ftr )
> Create a new transition on the heap to the state fst, with label c-addr u, number events n, action xt and data x
**ftr-free** ( ftr -- )
> Free the transition from the heap
#### Member words ####
**ftr-condition@** ( ftr -- bar )
> Get the condition of the transition as reference to a bit array
**ftr-label@** ( ftr -- c-addr u )
> Get the label of the transition
**ftr-label?** ( c-addr u ftr -- ftr true | c-addr u false )
> Check the label c-addr u with the transition ftr
**ftr-data@** ( ftr -- x )
> Get the data of the transition
**ftr-data!** ( x ftr -- )
> Set the data for the transition
**ftr-action@** ( ftr -- xt )
> Get the action of the transition
**ftr-attributes!** ( c-addr u ftr -- )
> Set the extra graphviz attributes for the transition
**ftr-attributes@** ( ftr -- c-addr u )
> Get the extra graphviz attributes of the transition
#### Event words ####
**ftr-fire** ( n ftr -- fst )
> Fire the transition for event n, without checking the condition
**ftr-feed** ( n ftr -- n false | fst true )
> Feed the event to this transition, return the next state or the event if the event did not match the condition
**ftr-try** ( n ftr -- n false | fst true )
> Try the event for this transition, return the result
#### Inspection ####
**ftr-dump** ( ftr -- )
> Dump the transition
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