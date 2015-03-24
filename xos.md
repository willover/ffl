### Module Description ###
The xos module implements an xml and html writer. The xos module extends
the tos module with extra words, so the xos words work on tos variables.
The module translates the normal entity references: &lt;, &gt;, &quot;,
&amp; and '. All other entity references should be written with the word
xos-write-raw-text. Note: balancing of start and end tags is not checked,
so the module can also be used to write html output.

### Module Words ###
#### xml writer words ####
**xos-write-start-xml** ( c-addr1 u1 ... c-addr2n u2n n tos -- )
> Write the start of a xml document with n attributes and values
**xos-write-text** ( c-addr u tos -- )
> Write normal xml text c-addr u with translation to the default entity references
**xos-write-start-tag** ( c-addr1 u1 ... c-addr2n u2n n c-addr u tos -- )
> Write the xml start tag c-addr u with n attributes and values c-addr**u**
**xos-write-end-tag** ( c-addr u tos -- )
> Write the xml end tag c-addr u
**xos-write-empty-element** ( c-addr1 u1 ... c-addr2n u2n n c-addr u tos -- )
> Write the xml start and end tag c-addr u with n attributes and values c-addr**u**
**xos-write-raw-text** ( c-addr u tos -- )
> Write unprocessed xml text
**xos-write-comment** ( c-addr u tos -- )
> Write a xml comment
**xos-write-cdata** ( c-addr u tos -- )
> Write a xml CDATA section
**xos-write-proc-instr** ( c-addr1 u1 c-addr2n u2n n c-addr u tos -- )
> Write a xml processing instruction with target c-addr u and n attributes and values c-addr**u**
**xos-write-internal-dtd** ( c-addr1 u1 c-addr2 u2 tos -- )
> Write an internal document type definition with name c-addr2 u2 and markup c-addr1 u1
**xos-write-system-dtd** ( c-addr1 u1 c-addr2 u2 c-addr3 u3 tos -- )
> Write a system document type definition with name c-addr3 u3, markup c-addr2 u2 and system c-addr1 u1
**xos-write-public-dtd** ( c-addr1 u1 c-addr2 u2 c-addr3 u3 c-addr4 u4 tos -- )
> Write a public document type definition with name c-addr4 u4, markup c-addr3 u3, public-id c-addr2 u2 and system c-addr1 u1
### Examples ###
```
include ffl/xos.fs


\ Example 1: Write xml output


\ Create the xml output stream

tos-create xos1

s" encoding" s" ISO-8859-1" s" version" s" 1.0" 2 xos1 xos-write-start-xml

s"  menu of the day " xos1 xos-write-comment

0 s" menu"         xos1 xos-write-start-tag
0 s" food"         xos1 xos-write-start-tag
0 s" name"         xos1 xos-write-start-tag
  s" Breakfast"    xos1 xos-write-text
  s" name"         xos1 xos-write-end-tag
0 s" description"  xos1 xos-write-start-tag
  s" two eggs & bacon or sausage & toast & hash browns" xos1 xos-write-text
  s" description"  xos1 xos-write-end-tag
0 s" price"        xos1 xos-write-start-tag
  s" $6.95"        xos1 xos-write-text
  s" price"        xos1 xos-write-end-tag
s" cholesterol" s" yes"
1 s" health"       xos1 xos-write-empty-element
  s" food"         xos1 xos-write-end-tag
  s" menu"         xos1 xos-write-end-tag


xos1 str-get type cr                 \ Type the xml info


\ Example 2: Write html output


xos1 tos-rewrite                     \ Clean the output stream

nil 0 s" -//W3C//DTD HTML 4.0 Transitional//EN" nil 0 s" HTML" xos1 xos-write-public-dtd

0 s" HTML"         xos1 xos-write-start-tag
0 s" HEAD"         xos1 xos-write-start-tag
0 s" TITLE"        xos1 xos-write-start-tag
  s" HTML example" xos1 xos-write-text
  s" TITLE"        xos1 xos-write-end-tag
0 s" BODY"         xos1 xos-write-start-tag
0 s" H1"           xos1 xos-write-start-tag
  s" 'Welcome'"    xos1 xos-write-text          \ Translate ' to &apos;
  s" H1"           xos1 xos-write-end-tag
  s"  comment "    xos1 xos-write-comment
  
  s" align" s" center"
1 s" DIV"          xos1 xos-write-start-tag     \ = <DIV align="center">

  s" Test"         xos1 xos-write-text
  s" DIV"          xos1 xos-write-end-tag
  s" BODY"         xos1 xos-write-end-tag
  s" HTML"         xos1 xos-write-end-tag
  
xos1 str-get type cr
```

---

Generated by **ofcfrth-0.10.0**