## ANS Labelling ##

The Forth Foundation Library is written in ANS-standard forth. But it requires some
extensions. All those extensions are listed in one config source file which is forth
engine specific. One exception is the word: `include` _filename_. This word
is used to include all modules. This is not a big problem, because this word is
quite common in forth engines.

FFL is labelled as follows in accordance with the ANS standard:

  * ANS Forth Program with Environmental Dependencies.
  * Requiring names from the Core Extensions word set.
  * Requiring the Double-Number word set.
  * Requiring names from the Double-Number Extensions word set.
  * Requiring the Exception word set.
  * Requiring the Facility word set.
  * Requiring names from the Facility Extensions word set.
  * Requiring the File Access word set.
  * Requiring names from the File Access Extensions word set.
  * Requiring (optional) the Floating-Point word set.
  * Requiring (optional) names from the Floating-Point Extensions word set.
  * Requiring the Memory-Allocation word set.
  * Requiring the Programming-Tools word set.
  * Requiring names from the Programming-Tools Extensions word set.
  * Requiring the String word set.

> ![http://c14.statcounter.com/1434121/0/407ca2a5/0/?img=stats.png](http://c14.statcounter.com/1434121/0/407ca2a5/0/?img=stats.png)