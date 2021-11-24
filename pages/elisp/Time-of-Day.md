

### 40.5 Time of Day

This section explains how to determine the current time and time zone.

Many functions like `current-time` and `file-attributes` return *Lisp timestamp* values that count seconds, and that can represent absolute time by counting seconds since the *epoch* of 1970-01-01 00:00:00 UTC.

Although traditionally Lisp timestamps were integer pairs, their form has evolved and programs ordinarily should not depend on the current default form. If your program needs a particular timestamp form, you can use the `time-convert` function to convert it to the needed form. See [Time Conversion](Time-Conversion.html).

There are currently three forms of Lisp timestamps, each of which represents a number of seconds:

An integer. Although this is the simplest form, it cannot represent subsecond timestamps.

A pair of integers `(ticks . hz)`, where `hz` is positive. This represents `ticks`/`hz` seconds, which is the same time as plain `ticks` if `hz` is 1. A common value for `hz` is 1000000000, for a nanosecond-resolution clock.[24](#FOOT24)

A list of four integers `(high low micro pico)`, where 0≤`low`\\<65536, 0≤`micro`\\<1000000, and 0≤`pico`\\<1000000. This represents the number of seconds using the formula: `high` \* 2\*\*16 + `low` + `micro` \* 10\*\*-6 + `pico` \* 10\*\*-12. In some cases, functions may default to returning two- or three-element lists, with omitted `micro` and `pico` components defaulting to zero. On all current machines `pico` is a multiple of 1000, but this may change as higher-resolution clocks become available.

Function arguments, e.g., the `time` argument to `current-time-string`, accept a more-general *time value* format, which can be a Lisp timestamp, `nil` for the current time, a single floating-point number for seconds, or a list `(high low micro)` or `(high low)` that is a truncated list timestamp with missing elements taken to be zero.

Time values can be converted to and from calendrical and other forms. Some of these conversions rely on operating system functions that limit the range of possible time values, and signal an error such as ‘`"Specified time is not representable"`’ if the limits are exceeded. For instance, a system may not support years before 1970, or years before 1901, or years far in the future. You can convert a time value into a human-readable string using `format-time-string`, into a Lisp timestamp using `time-convert`, and into other forms using `decode-time` and `float-time`. These functions are described in the following sections.

### Function: **current-time-string** *\&optional time zone*

This function returns the current time and date as a human-readable string. The format does not vary for the initial part of the string, which contains the day of week, month, day of month, and time of day in that order: the number of characters used for these fields is always the same, although (unless you require English weekday or month abbreviations regardless of locale) it is typically more convenient to use `format-time-string` than to extract fields from the output of `current-time-string`, as the year might not have exactly four digits, and additional information may some day be added at the end.

The argument `time`, if given, specifies a time to format, instead of the current time. The optional argument `zone` defaults to the current time zone rule. See [Time Zone Rules](Time-Zone-Rules.html). The operating system limits the range of time and zone values.

```lisp
(current-time-string)
     ⇒ "Fri Nov  1 15:59:49 2019"
```

### Function: **current-time**

This function returns the current time as a Lisp timestamp. Although the timestamp takes the form `(high low micro pico)` in the current Emacs release, this is planned to change in a future Emacs version. You can use the `time-convert` function to convert a timestamp to some other form. See [Time Conversion](Time-Conversion.html).

### Function: **float-time** *\&optional time*

This function returns the current time as a floating-point number of seconds since the epoch. The optional argument `time`, if given, specifies a time to convert instead of the current time.

*Warning*: Since the result is floating point, it may not be exact. Do not use this function if precise time stamps are required. For example, on typical systems `(float-time '(1 . 10))` displays as ‘`0.1`’ but is slightly greater than 1/10.

`time-to-seconds` is an alias for this function.

***

#### Footnotes

##### [(24)](#DOCF24)

Currently `hz` should be at least 65536 to avoid compatibility warnings when the timestamp is passed to standard functions, as previous versions of Emacs would interpret such a timestamps differently due to backward-compatibility concerns. These warnings are intended to be removed in a future Emacs version.
