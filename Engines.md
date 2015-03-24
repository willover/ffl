﻿#summary Forth engines support in the FFL
#labels Featured

## Supported Forth engines ##

FFL will run on and is tested with the following forth engines :

| **Engine** | **Since** |
|:-----------|:----------|
| [GForth](http://www.jwdt.com/~paysan/gforth.html) | 0.1.0 |
| [BigForth](http://www.jwdt.com/~paysan/bigforth.html) | 0.2.0 |
| [PFE](http://pfe.sourceforge.net) | 0.3.0 |
| [Win32Forth](http://win32forth.sourceforge.net) | 0.4.0 |
| [fina](http://code.google.com/p/fina-forth) | 0.5.0 |
| [MinForth](http://home.arcor.de/a.s.kochenburger/minforth.html) | 0.5.0 |
| [iForth](http://home.iae.nl/users/mhx) | 0.6.0 |
| [SP-Forth](http://spf.sourceforge.net) | 0.7.0 |
| [lxf/ntf](http://falth.homelinux.net/readme2.html) | 0.7.0 |
| [VFXForth](http://www.mpeforth.com) | 0.8.0 |
| [SwiftForth](http://www.forth.com/swiftforth/index.htm) | 0.9.0 |

For these forth engines there is a config file present in the distribution package.

## Unsupported Forth Engines ##

FFL will not run on the following forth engines :

| **Engine** | **Version** | **Reason** |
|:-----------|:------------|:-----------|
| kForth  |  | Missing ANS words |
| ciforth | 4.0.6 | Missing ANS words |
| pforth |  | Missing ANS words |
| isforth |  | Missing ANS words |
|  4th |  | Missing ANS words |
| helforth | 2.51 | Missing ANS words |

## Not yet checked Forth Engines ##

  * RetroForth
  * ficl
  * [forth4p](http://code.google.com/p/forth4p/)
  * ...

## Environmental dependencies ##

A few modules have environmental dependencies. As a result they will not be present
for some forth engines. For these modules you should test the presence of the `version`
constant after loading the module (see [Getting started](Using.md)).

|                                           | **GForth** | **BigForth** | **PFE** | **Win32Forth** | **MinForth** | **iForth** | **SP-Forth** | **lxf/ntf** | **VFXForth** |
|:------------------------------------------|:-----------|:-------------|:--------|:---------------|:-------------|:-----------|:-------------|:------------|:-------------|
| Argument Parser (arg)                     | Yes      | No         | Yes   | No           | No         | No       | No         | No        | No         |
| Distributed Random Number Generator (rdg) | Yes      | No         | Yes   | Yes          | Yes        | Yes      | No         | No        | No         |
| Timer (tmr)                               | Yes      | Yes        | Yes   | Yes          | Yes        | Yes      | Yes        | Yes       | Yes        |
| GTK-server interface (gsv)                | Yes      | No         | Yes   | No           | No         | ?        | Yes        | Yes       | Yes        |

Some notes for the GTK-server interface:
  * gsv is for the moment only tested on Linux.
  * PFE will sometimes get stuck or report a broken pipe.
  * BigForth and MinForth have problems with the long names of the GTK functions.

## Benchmarks ##

### Version 0.8.0 ###

|                      | **GForth** | **BigForth** | **PFE**  | **Win32Forth** | **MinForth** | **SP-Forth** | **lxf**  | **VFXForth** |
|:---------------------|:-----------|:-------------|:---------|:---------------|:-------------|:-------------|:---------|:-------------|
| Compile Time (msec)  | 1087     | 251        | 459    | 1467         | 4280       | 2680       | 344    | 500        |
| Compile Size (bytes) | 247784   | 210820     | 222204 | 218196       | 231732     | 214777     | 211599 | 356392     |
| Test Time (sec)      | 4.2      | 2.9        | 11.2   | 8.7          | 48.6       | 5.6        | 2.1    | 2.1        |

### Version 0.7.0 ###

|                      | **GForth** | **BigForth** | **PFE**  | **Win32Forth** | **MinForth** | **SP-Forth** | **lxf**  |
|:---------------------|:-----------|:-------------|:---------|:---------------|:-------------|:-------------|:---------|
| Compile Time (msec)  | 850      | 328        | 517    | 1248         | 3780       | 2530       | 310    |
| Compile Size (bytes) | 224912   | 196089     | 209764 | 203748       | 215676     | 201260     | 206142 |
| Test Time (sec)      | 4.1      | 3.4        | 11.2   | 11.3         | 49.0       | 5.6        | 2.0    |

### Version 0.6.0 ###

|                      | **GForth** | **BigForth** | **PFE**  | **Win32Forth** | **MinForth** |
|:---------------------|:-----------|:-------------|:---------|:---------------|:-------------|
| Compile Time (msec)  | 1774     | 352        | 720    | 2630         | 9440       |
| Compile Size (bytes) | 169560   | 146406     | 166704 | 97176        | 106600     |
| Test Time (sec)      | 7.9      | 8.2        | 24.1   | 15.3         | 140.2      |

### Version 0.5.0 ###

|                      | **GForth** | **BigForth** | **PFE**  | **Win32Forth** | **MinForth** |
|:---------------------|:-----------|:-------------|:---------|:---------------|:-------------|
| Compile Time (msec)  | 1020     | 220        | 374    | 1760         | 6100       |
| Compile Size (bytes) | 132724   | 115090     | 133796 | 75696        | 83128      |
| Test Time (sec)      | 6.8      | 8.2        | 23.6   | 14.3         | 132.9      |

### Version 0.4.0 ###

|                      | **GForth** | **BigForth** | **PFE**  | **Win32Forth** |
|:---------------------|:-----------|:-------------|:---------|:---------------|
| Compile Time (msec)  | 690      | 158        | 255    | 1200         |
| Compile Size (bytes) | 103132   | 89710      | 112628 | 59660        |
| Test Time (sec)      | 3.8      | 6.2        | 9.2    | 9.6          |

Legenda:

  * The Compile Time is the time required to include all modules in the dictionary.
  * The Compile Size is the space required in the dictionary to include all modules.
  * The Test time is the time required to include and run all the test scripts.

Notes:

  * Benchmarks for versions 0.4.0 till 0.6.0 were done on a Celeron 700 MHZ.
  * Benchmarks for version 0.7.0 and higher were done on an Athlon 64 X2 1800MHZ

## Porting ##

As already stated the FFL is mostly written in ANS forth. If the forth engine you are using, is
not listed on the [Engines](Engines.md) page and it conforms to the ANS-standard, you can try porting ffl
to your forth engine by adding words to and removing words from the `ffl/config.fs` file.
If you succeed, you can send me the config.fs file so that I can add it to the distribution.

![http://c14.statcounter.com/1434121/0/407ca2a5/0/?img=stats.png](http://c14.statcounter.com/1434121/0/407ca2a5/0/?img=stats.png)