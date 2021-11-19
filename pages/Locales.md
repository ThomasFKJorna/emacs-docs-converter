

Previous: [Input Methods](Input-Methods.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 33.12 Locales

In POSIX, locales control which language to use in language-related features. These Emacs variables control how Emacs interacts with these features.

*   Variable: **locale-coding-system**

    This variable specifies the coding system to use for decoding system error messages and—on X Window system only—keyboard input, for sending batch output to the standard output and error streams, for encoding the format argument to `format-time-string`, and for decoding the return value of `format-time-string`.

<!---->

*   Variable: **system-messages-locale**

    This variable specifies the locale to use for generating system error messages. Changing the locale can cause messages to come out in a different language or in a different orthography. If the variable is `nil`, the locale is specified by environment variables in the usual POSIX fashion.

<!---->

*   Variable: **system-time-locale**

    This variable specifies the locale to use for formatting time values. Changing the locale can cause messages to appear according to the conventions of a different language. If the variable is `nil`, the locale is specified by environment variables in the usual POSIX fashion.

<!---->

*   Function: **locale-info** *item*

    This function returns locale data `item` for the current POSIX locale, if available. `item` should be one of these symbols:

    *   `codeset`

        Return the character set as a string (locale item `CODESET`).

    *   `days`

        Return a 7-element vector of day names (locale items `DAY_1` through `DAY_7`);

    *   `months`

        Return a 12-element vector of month names (locale items `MON_1` through `MON_12`).

    *   `paper`

        Return a list `(width height)` of 2 integers, for the default paper size measured in millimeters (locale items `_NL_PAPER_WIDTH` and `_NL_PAPER_HEIGHT`).

    If the system can’t provide the requested information, or if `item` is not one of those symbols, the value is `nil`. All strings in the return value are decoded using `locale-coding-system`. See [Locales](https://www.gnu.org/software/libc/manual/html_node/Locales.html#Locales) in The GNU Libc Manual, for more information about locales and locale items.

Previous: [Input Methods](Input-Methods.html), Up: [Non-ASCII Characters](Non_002dASCII-Characters.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
