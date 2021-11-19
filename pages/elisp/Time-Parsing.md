<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Processor Run Time](Processor-Run-Time.html), Previous: [Time Conversion](Time-Conversion.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.8 Parsing and Formatting Times

These functions convert time values to text in a string, and vice versa. Time values include `nil`, numbers, and Lisp timestamps (see [Time of Day](Time-of-Day.html)).

*   Function: **date-to-time** *string*

    This function parses the time-string `string` and returns the corresponding Lisp timestamp. The argument `string` should represent a date-time, and should be in one of the forms recognized by `parse-time-string` (see below). This function assumes Universal Time if `string` lacks explicit time zone information. The operating system limits the range of time and zone values.

<!---->

*   Function: **parse-time-string** *string*

    This function parses the time-string `string` into a list of the following form:

        (sec min hour day mon year dow dst tz)

    The format of this list is the same as what `decode-time` accepts (see [Time Conversion](Time-Conversion.html)), and is described in more detail there. Any element that cannot be determined from the input will be set to `nil`. The argument `string` should resemble an RFC 822 (or later) or ISO 8601 string, like “Fri, 25 Mar 2016 16:24:56 +0100” or “1998-09-12T12:21:54-0200”, but this function will attempt to parse less well-formed time strings as well.

<!---->

*   Function: **iso8601-parse** *string*

    For a more strict function (that will error out upon invalid input), this function can be used instead. It can parse all variants of the ISO 8601 standard, so in addition to the formats mentioned above, it also parses things like “1998W45-3” (week number) and “1998-245” (ordinal day number). To parse durations, there’s `iso8601-parse-duration`, and to parse intervals, there’s `iso8601-parse-interval`. All these functions return decoded time structures, except the final one, which returns three of them (the start, the end, and the duration).

<!---->

*   Function: **format-time-string** *format-string \&optional time zone*

    This function converts `time` (or the current time, if `time` is omitted or `nil`) to a string according to `format-string`. The conversion uses the time zone rule `zone`, which defaults to the current time zone rule. See [Time Zone Rules](Time-Zone-Rules.html). The argument `format-string` may contain ‘`%`’-sequences which say to substitute parts of the time. Here is a table of what the ‘`%`’-sequences mean:

    *   ‘`%a`’

        This stands for the abbreviated name of the day of week.

    *   ‘`%A`’

        This stands for the full name of the day of week.

    *   ‘`%b`’

        This stands for the abbreviated name of the month.

    *   ‘`%B`’

        This stands for the full name of the month.

    *   ‘`%c`’

        This is a synonym for ‘`%x %X`’.

    *   ‘`%C`’

        This stands for the century, that is, the year divided by 100, truncated toward zero. The default field width is 2.

    *   ‘`%d`’

        This stands for the day of month, zero-padded.

    *   ‘`%D`’

        This is a synonym for ‘`%m/%d/%y`’.

    *   ‘`%e`’

        This stands for the day of month, blank-padded.

    *   ‘`%F`’

        This stands for the ISO 8601 date format, which is like ‘`%+4Y-%m-%d`’ except that any flags or field width override the ‘`+`’ and (after subtracting 6) the ‘`4`’.

    *   ‘`%g`’

        This stands for the year corresponding to the ISO week within the century.

    *   ‘`%G`’

        This stands for the year corresponding to the ISO week.

    *   ‘`%h`’

        This is a synonym for ‘`%b`’.

    *   ‘`%H`’

        This stands for the hour (00–23).

    *   ‘`%I`’

        This stands for the hour (01–12).

    *   ‘`%j`’

        This stands for the day of the year (001–366).

    *   ‘`%k`’

        This stands for the hour (0–23), blank padded.

    *   ‘`%l`’

        This stands for the hour (1–12), blank padded.

    *   ‘`%m`’

        This stands for the month (01–12).

    *   ‘`%M`’

        This stands for the minute (00–59).

    *   ‘`%n`’

        This stands for a newline.

    *   ‘`%N`’

        This stands for the nanoseconds (000000000–999999999). To ask for fewer digits, use ‘`%3N`’ for milliseconds, ‘`%6N`’ for microseconds, etc. Any excess digits are discarded, without rounding.

    *   ‘`%p`’

        This stands for ‘`AM`’ or ‘`PM`’, as appropriate.

    *   ‘`%q`’

        This stands for the calendar quarter (1–4).

    *   ‘`%r`’

        This is a synonym for ‘`%I:%M:%S %p`’.

    *   ‘`%R`’

        This is a synonym for ‘`%H:%M`’.

    *   ‘`%s`’

        This stands for the integer number of seconds since the epoch.

    *   ‘`%S`’

        This stands for the second (00–59, or 00–60 on platforms that support leap seconds).

    *   ‘`%t`’

        This stands for a tab character.

    *   ‘`%T`’

        This is a synonym for ‘`%H:%M:%S`’.

    *   ‘`%u`’

        This stands for the numeric day of week (1–7). Monday is day 1.

    *   ‘`%U`’

        This stands for the week of the year (01–52), assuming that weeks start on Sunday.

    *   ‘`%V`’

        This stands for the week of the year according to ISO 8601.

    *   ‘`%w`’

        This stands for the numeric day of week (0–6). Sunday is day 0.

    *   ‘`%W`’

        This stands for the week of the year (01–52), assuming that weeks start on Monday.

    *   ‘`%x`’

        This has a locale-specific meaning. In the default locale (named ‘`C`’), it is equivalent to ‘`%D`’.

    *   ‘`%X`’

        This has a locale-specific meaning. In the default locale (named ‘`C`’), it is equivalent to ‘`%T`’.

    *   ‘`%y`’

        This stands for the year without century (00–99).

    *   ‘`%Y`’

        This stands for the year with century.

    *   ‘`%Z`’

        This stands for the time zone abbreviation (e.g., ‘`EST`’).

    *   ‘`%z`’

        This stands for the time zone numerical offset. The ‘`z`’ can be preceded by one, two, or three colons; if plain ‘`%z`’ stands for ‘`-0500`’, then ‘`%:z`’ stands for ‘`-05:00`’, ‘`%::z`’ stands for ‘`-05:00:00`’, and ‘`%:::z`’ is like ‘`%::z`’ except it suppresses trailing instances of ‘`:00`’ so it stands for ‘`-05`’ in the same example.

    *   ‘`%%`’

        This stands for a single ‘`%`’.

    One or more flag characters can appear immediately after the ‘`%`’. ‘`0`’ pads with zeros, ‘`+`’ pads with zeros and also puts ‘`+`’ before nonnegative year numbers with more than four digits, ‘`_`’ pads with blanks, ‘`-`’ suppresses padding, ‘`^`’ upper-cases letters, and ‘`#`’ reverses the case of letters.

    You can also specify the field width and type of padding for any of these ‘`%`’-sequences. This works as in `printf`: you write the field width as digits in a ‘`%`’-sequence, after any flags. For example, ‘`%S`’ specifies the number of seconds since the minute; ‘`%03S`’ means to pad this with zeros to 3 positions, ‘`%_3S`’ to pad with spaces to 3 positions. Plain ‘`%3S`’ pads with zeros, because that is how ‘`%S`’ normally pads to two positions.

    The characters ‘`E`’ and ‘`O`’ act as modifiers when used after any flags and field widths in a ‘`%`’-sequence. ‘`E`’ specifies using the current locale’s alternative version of the date and time. In a Japanese locale, for example, `%Ex` might yield a date format based on the Japanese Emperors’ reigns. ‘`E`’ is allowed in ‘`%Ec`’, ‘`%EC`’, ‘`%Ex`’, ‘`%EX`’, ‘`%Ey`’, and ‘`%EY`’.

    ‘`O`’ means to use the current locale’s alternative representation of numbers, instead of the ordinary decimal digits. This is allowed with most letters, all the ones that output numbers.

    To help debug programs, unrecognized ‘`%`’-sequences stand for themselves and are output as-is. Programs should not rely on this behavior, as future versions of Emacs may recognize new ‘`%`’-sequences as extensions.

    This function uses the C library function `strftime` (see [Formatting Calendar Time](https://www.gnu.org/software/libc/manual/html_node/Formatting-Calendar-Time.html#Formatting-Calendar-Time) in The GNU C Library Reference Manual) to do most of the work. In order to communicate with that function, it first converts `time` and `zone` to internal form; the operating system limits the range of time and zone values. This function also encodes `format-string` using the coding system specified by `locale-coding-system` (see [Locales](Locales.html)); after `strftime` returns the resulting string, this function decodes the string using that same coding system.

<!---->

*   Function: **format-seconds** *format-string seconds*

    This function converts its argument `seconds` into a string of years, days, hours, etc., according to `format-string`. The argument `format-string` may contain ‘`%`’-sequences which control the conversion. Here is a table of what the ‘`%`’-sequences mean:

    *   *   ‘`%y`’
        *   ‘`%Y`’

        The integer number of 365-day years.

    *   *   ‘`%d`’
        *   ‘`%D`’

        The integer number of days.

    *   *   ‘`%h`’
        *   ‘`%H`’

        The integer number of hours.

    *   *   ‘`%m`’
        *   ‘`%M`’

        The integer number of minutes.

    *   *   ‘`%s`’
        *   ‘`%S`’

        The integer number of seconds.

    *   ‘`%z`’

        Non-printing control flag. When it is used, other specifiers must be given in the order of decreasing size, i.e., years before days, hours before minutes, etc. Nothing will be produced in the result string to the left of ‘`%z`’ until the first non-zero conversion is encountered. For example, the default format used by `emacs-uptime` (see [emacs-uptime](Processor-Run-Time.html)) `"%Y, %D, %H, %M, %z%S"`<!-- /@w --> means that the number of seconds will always be produced, but years, days, hours, and minutes will only be shown if they are non-zero.

    *   ‘`%%`’

        Produces a literal ‘`%`’.

    Upper-case format sequences produce the units in addition to the numbers, lower-case formats produce only the numbers.

    You can also specify the field width by following the ‘`%`’ with a number; shorter numbers will be padded with blanks. An optional period before the width requests zero-padding instead. For example, `"%.3Y"` might produce `"004 years"`.

Next: [Processor Run Time](Processor-Run-Time.html), Previous: [Time Conversion](Time-Conversion.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
