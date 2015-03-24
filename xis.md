### Module Description ###
The xis module implements a non-validating XML/HTML parser.
The default entity references, &lt; &gt; &amp;, &quot; and ' are
translated, all others are simply returned in the text. By using the
xis-msc! word a message catalog can be set, that will overrule the default
translations of entity references. The xis-set-reader word expects an
execution token with the following stack behavior:
```
   x -- c-addr u | 0
```
Data x is the same as the first parameter during calling of the word
xis-set-reader. For reading from files this is normally the file
descriptor. The word returns, if successful, the read data in c-addr u.
The xis-read word returns the parsed xml token with the following varying
stack parameters:
```
xis.error          --
xis.done           --
xis.start-xml      -- c-addr1 u1 .. c-addrn un n          = Return n attribute names with their value
xis.comment        -- c-addr u                            = Return the comment
xis.text           -- c-addr u                            = Return the normal text
xis.start-tag      -- c-addr1 u1 .. c-addrn un n c-addr u = Return the tag name and n attributes with their value
xis.end-tag        -- c-addr u                            = Return the tag name
xis.empty-element  -- c-addr1 u1 .. c-addrn un n c-addr u = Return the tag name and n attributes with their value
xis.cdata          -- c-addr u                            = Return the CDATA section text
xis.proc-instr     -- c-addr1 u1 .. c-addrn un n c-addr u = Return the target name and n attributes with their value
xis.internal-dtd   -- c-addr1 u1 c-addr2 u2               = Return the DTD name c-addr2 u2 and markup c-addr1 u1
xis.public-dtd     -- c-addr1 u1 c-addr2 u2 c-addr3 u3 c-addr4 u4 = Return the DTD name, the markup, the system-id and public-id
xis.system-dtd     -- c-addr1 u1 c-addr2 u2 c-addr3 u3    = Return the DTD name, the markup and the system-id
```

### Module Words ###
#### xis reader constants ####
**xis.error** ( -- n )
> Error
**xis.done** ( -- n )
> Done reading
**xis.start-xml** ( -- n )
> Start Document
**xis.comment** ( -- n )
> Comment
**xis.text** ( -- n )
> Normal text
**xis.start-tag** ( -- n )
> Start tag
**xis.end-tag** ( -- n )
> End tag
**xis.empty-element** ( -- n )
> Empty element
**xis.cdata** ( -- n )
> CDATA section
**xis.proc-instr** ( -- n )
> Proc. instr.
**xis.internal-dtd** ( -- n )
> Internal DTD
**xis.public-dtd** ( -- n )
> Public DTD
**xis.system-dtd** ( -- n )
> System DTD
#### xml reader structure ####
**xis%** ( -- n )
> Get the required space for a xis reader variable
#### xml reader variable creation, initialisation and destruction ####
**xis-init** ( xis -- )
> Initialise the xml reader variable
**xis-(free)** ( xis -- )
> Free the internal, private variables from the heap
**xis-create** ( "`<`spaces`>`name" -- ; -- xis )
> Create a named xml reader variable in the dictionary
**xis-new** ( -- xis )
> Create a new xml reader variable on the heap
**xis-free** ( xis -- )
> Free the xis reader variable from the heap
#### xml reader init words ####
**xis-set-reader** ( x xt xis -- )
> Init the xml parser for reading using the reader callback xt with its data x
**xis-set-string** ( c-addr u xis -- )
> Init the xml parser for for reading from the string c-addr u
#### Member words ####
**xis-msc@** ( xis -- msc )
> Get the current entity reference catalog
**xis-msc!** ( msc xis -- )
> Set the entity reference catalog for the reader
**xis-strip@** ( xis -- flag )
> Return flag indicating the stripping of leading and trailing white space in normal text
**xis-strip!** ( flag xis -- )
> Set the flag indicating the stripping of leading and trailing white space in normal text
#### xml reader word ####
**xis-read** ( xis -- i\*x n )
> Read the next xml token n with various parameters from the source (see xml reader constants)
**xis+remove-read-parameters** ( i\*x n -- )
> Remove the various parameters of a xml token after calling xis-read (see xml reader constants)
**xis+dump-read-parameters** ( i\*x n -- )
> Dump the various parameters of a xml token after calling xis-read (see xml reader constants)
### Examples ###
```
include ffl/xis.fs


\ Example: Read a XML/HTML file

\ Create a XML/HTML input stream on the heap

xis-new value xis1


\ Setup the reader callback word for reading from file

: file-reader ( fileid -- c-addr u | 0 )
  pad 64 rot read-file throw
  dup IF
    pad swap
  THEN
;



s" test.xml" r/o open-file throw value xis.file  \ Open the file

xis.file  ' file-reader   xis1 xis-set-reader     \ Use the xml reader with a file

true xis1 xis-strip!                              \ Strip leading and trailing whitespace in the text

: file-parse  ( -- = Parse the xml file )
  BEGIN
    xis1 xis-read                           \ Read the next token from the file
    dup xis.error <> over xis.done <> AND   \ Done when ready or error
  WHILE
    xis+dump-read-parameters                \ Dump the next token with its parameters
   
  REPEAT
  
  xis.error = IF
    ." Error parsing the file." cr
  ELSE
    ." File succesfully parsed." cr
  THEN
;

\ Parse the file

file-parse


\ Done, close the file

xis.file close-file throw


\ Free the stream from the heap

xis1 xis-free

```

---

Generated by **ofcfrth-0.10.0**