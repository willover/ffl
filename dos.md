### Module Description ###
The dos module implements a date and time formatter. It is built upon the
tos structure, so all words write to the tos stream. If the tos structure
contains a message catalog, it is used for localization of times, dates
and names. The format word uses most of the same conversion characters as
the strftime c-function:
```
%a - the abbreviated weekday name using the streams catalog for locale
%A - the full weekday name using the streams catalog for locale
%b - the abbreviated month name using the streams catalog for locale
%B - the full month name using the streams catalog for locale
%c - the preferred date and time using the streams catalog for locale
%C - the century number as a 2-digit integer
%d - the day of the month as a decimal number: 00..31
%D - equivalent to %m/%d/%y -- for Americans
%e - like %d, the day of the month as a decimal number with leading space
%F - equivalent to %Y-%m-%d, the ISO 8601 date format
%h - equivalent to %b
%H - the hour as a decimal number using a 24-hour clock: 00..23
%I - the hour as a decimal number using a 12-hour clock: 01..12
%j - the day of the year as a decimal number: 001..366
%k - the hour, 24-hour clock, as a decimal number: 0..23, leading space
%l - the hour, 12-hour clock, as a decimal number: 1..12, leading space
%m - the month as a decimal number: 01..12
%M - the minute as a decimal number: 00..59
%p - either 'AM' or 'PM' according to the given time value with locale
%P - like %p but in lowercase: 'am' with locale
%r - the time in a.m. or p.m. notation, '%I:%M:%S %p'
%R - the time in 24-hour notation, %H:%M
%s - the number of seconds since the 1970-01-01 00:00:00
%S - the second as a decimal number: 00..60
%T - the time in 24-hour notation, %H:%M:%S
%V - the ISO 8601 week number as a decimal number: 01..53
%w - the day of the week as a decimal, range 0 to 6, Sunday being 0
%x - the preferred date using the streams catalog for locale
%X - the preferred time using the streams catalog for locale
%y - the year as a decimal number without a century: 00..99
%Y - the year as a decimal number including the century
%% - the character %
```

### Module Words ###
#### Date and time writing words ####
**dos-write-abbr-weekday-name** ( dtm tos -- )
> Write the abbreviated weekday name, using the streams catalog for locale
**dos-write-weekday-name** ( dtm tos -- )
> Write the full weekday name, using the streams catalog for locale
**dos-write-abbr-month-name** ( dtm tos -- )
> Write the abbreviated month name, using the streams catalog for locale
**dos-write-month-name** ( dtm tos -- )
> Write the full month name, using the streams catalog for locale
**dos-write-date-time** ( dtm tos -- )
> Write the preferred time and date using the streams catalog for the locale, else yyyy/mm/dd hh:mm:ss
**dos-write-century** ( dtm tos -- )
> Write the century number
**dos-write-monthday** ( dtm tos -- )
> Write the day of the month: 01..31
**dos-write-american-date** ( dtm tos -- )
> Write the date in mm/dd/yy format
**dos-write-spaced-monthday** ( dtm tos -- )
> Write the day of the month,  1..31, space padded
**dos-write-iso8601-date** ( dtm tos -- )
> Write the date in ISO 8601 format: yyyy-mm-dd
**dos-write-24hour** ( dtm tos -- )
> Write the hour using a 24-hour clock: 00..23
**dos-write-12hour** ( dtm tos -- )
> Write the hour using a 12-hour clock: 01..12
**dos-write-yearday** ( dtm tos -- )
> Write the day of the year: 001..366
**dos-write-spaced-24hour** ( dtm tos -- )
> Write the hour using a 24-hour clock:  0..23, space padded
**dos-write-spaced-12hour** ( dtm tos -- )
> Write the hour using a 12-hour clock:  1..12, space padded
**dos-write-month** ( dtm tos -- )
> Write the month: 01..12
**dos-write-minute** ( dtm tos -- )
> Write the minute: 00..59
**dos-write-ampm** ( dtm tos -- )
> Write the am or pm notation, using the streams catalog for locale
**dos-write-upper-ampm** ( dtm tos -- )
> Write the AM or PM notation, using the streams catalog for locale
**dos-write-ampm-time** ( dtm tos -- )
> Write the time in ampm notation: hh:mm:ss ?m, using the streams catalog for locale
**dos-write-hhmm-time** ( dtm tos -- )
> Write the time: hh:mm
**dos-write-seconds-since-epoch** ( dtm tos -- )
> Write the number of seconds since 1970-01-01 00:00:00
**dos-write-seconds** ( dtm tos -- )
> Write the number of seconds: 00..61
**dos-write-hhmmss-time** ( dtm tos -- )
> Write the time: hh:mm:ss
**dos-write-weekday** ( dtm tos -- )
> Write the weekday: 0..6, 0 = sunday
**dos-write-week-number** ( dtm tos -- )
> Write the week number: 01..53
**dos-write-date** ( dtm tos -- )
> Write the preferred date using the streams catalog for the locale, else yyyy/mm/dd
**dos-write-time** ( dtm tos -- )
> Write the preferred time using the stream catalog for the locale, else hh:mm:ss
**dos-write-2year** ( dtm tos -- )
> Write the year without the century: 00..99
**dos-write-year** ( dtm tos -- )
> Write the year including the century
#### Date and time formatting word ####
**dos-write-format** ( dtm c-addr u tos -- )
> Write date and time info with the format string c-addr u in the stream tos
### Examples ###
```
include ffl/dos.fs
include ffl/gmo.fs



\ Example 1: Format the output with words

\ Create the date and output stream in the dictionary

dtm-create dtm1
tos-create tos1

\ Set the date with 22 november 2007 15:00:59

0 59 00 15 22 dtm.may 2007 dtm1 dtm-set

\ Format the output stream

dtm1 tos1 dos-write-date-time

tos1 str-get type cr                 \ Shows: 2007/05/22 15:00:59

tos1 tos-rewrite                     \ Clean the stream

dtm1 tos1 dos-write-weekday-name

tos1 str-get type cr                 \ Shows: Tuesday



\ Example 2: Format the output with a string format

tos1 tos-rewrite

dtm1 s" %a %b %e %H:%M:%S %Y" tos1 dos-write-format

tos1 str-get type cr                 \ Shows: Tue May 22 15:00:59 2007



\ Example 3: Use a message catalog for localisation of dates

msc-create en>nl                     \ Create the message catalog

s" nl.mo" en>nl gmo-read throw       \ Import the nl.mo file in the catalog

en>nl tos1 tos-msc!                  \ Let the ouput stream use the message catalog


tos1 tos-rewrite                     \ Clear the output stream

dtm1 s" %a %b %e %H:%M:%S %Y" tos1 dos-write-format

tos1 str-get type cr                 \ Shows: din mei 22 15:00:59 2007 (dutch)
```

---

Generated by **ofcfrth-0.10.0**