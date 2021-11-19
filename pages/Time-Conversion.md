

Next: [Time Parsing](Time-Parsing.html), Previous: [Time Zone Rules](Time-Zone-Rules.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.7 Time Conversion

These functions convert time values (see [Time of Day](Time-of-Day.html)) to Lisp timestamps, or into calendrical information and vice versa.

Many 32-bit operating systems are limited to system times containing 32 bits of information in their seconds component; these systems typically handle only the times from 1901-12-13 20:45:52 through 2038-01-19 03:14:07 Universal Time. However, 64-bit and some 32-bit operating systems have larger seconds components, and can represent times far in the past or future.

Calendrical conversion functions always use the Gregorian calendar, even for dates before the Gregorian calendar was introduced. Year numbers count the number of years since the year 1 BC, and do not skip zero as traditional Gregorian years do; for example, the year number -37 represents the Gregorian year 38 BC.

*   Function: **time-convert** *time \&optional form*

    This function converts a time value into a Lisp timestamp.

    The optional `form` argument specifies the timestamp form to be returned. If `form` is the symbol `integer`, this function returns an integer count of seconds. If `form` is a positive integer, it specifies a clock frequency and this function returns an integer-pair timestamp `(ticks . form)`.[25](#FOOT25) If `form` is `t`, this function treats it as a positive integer suitable for representing the timestamp; for example, it is treated as 1000000000 if `time` is nil and the platform timestamp has nanosecond resolution. If `form` is `list`, this function returns an integer list `(high low micro pico)`. Although an omitted or `nil` `form` currently acts like `list`, this is planned to change in a future Emacs version, so callers requiring list timestamps should pass `list` explicitly.

    If `time` is infinite or a NaN, this function signals an error. Otherwise, if `time` cannot be represented exactly, conversion truncates it toward minus infinity. When `form` is `t`, conversion is always exact so no truncation occurs, and the returned clock resolution is no less than that of `time`. By way of contrast, `float-time` can convert any Lisp time value without signaling an error, although the result might not be exact. See [Time of Day](Time-of-Day.html).

    For efficiency this function might return a value that is `eq` to `time`, or that otherwise shares structure with `time`.

    Although `(time-convert nil nil)` is equivalent to `(current-time)`, the latter may be a bit faster.

    ```lisp
    (setq a (time-convert nil t))
    ⇒ (1564826753904873156 . 1000000000)
    ```

    ```lisp
    (time-convert a 100000)
    ⇒ (156482675390487 . 100000)
    ```

    ```lisp
    (time-convert a 'integer)
    ⇒ 1564826753
    ```

    ```lisp
    (time-convert a 'list)
    ⇒ (23877 23681 904873 156000)
    ```

<!---->

*   Function: **decode-time** *\&optional time zone form*

    This function converts a time value into calendrical information. If you don’t specify `time`, it decodes the current time, and similarly `zone` defaults to the current time zone rule. See [Time Zone Rules](Time-Zone-Rules.html). The operating system limits the range of time and zone values.

    The `form` argument controls the form of the returned `seconds` element, as described below. The return value is a list of nine elements, as follows:

    ```lisp
    (seconds minutes hour day month year dow dst utcoff)
    ```

    Here is what the elements mean:

    *   `seconds`

        The number of seconds past the minute, with form described below.

    *   `minutes`

        The number of minutes past the hour, as an integer between 0 and 59.

    *   `hour`

        The hour of the day, as an integer between 0 and 23.

    *   `day`

        The day of the month, as an integer between 1 and 31.

    *   `month`

        The month of the year, as an integer between 1 and 12.

    *   `year`

        The year, an integer typically greater than 1900.

    *   `dow`

        The day of week, as an integer between 0 and 6, where 0 stands for Sunday.

    *   `dst`

        `t` if daylight saving time is effect, `nil` if it is not in effect, and -1 if this information is not available.

    *   `utcoff`

        An integer indicating the Universal Time offset in seconds, i.e., the number of seconds east of Greenwich.

    The `seconds` element is a Lisp timestamp that is nonnegative and less than 61; it is less than 60 except during positive leap seconds (assuming the operating system supports leap seconds). If the optional `form` argument is `t`, `seconds` uses the same precision as `time`; if `form` is `integer`, `seconds` is truncated to an integer. For example, if `time` is the timestamp `(1566009571321 . 1000)`, which represents 2019-08-17 02:39:31.321 UTC on typical systems that lack leap seconds, then `(decode-time time t t)` returns `((31321 . 1000) 39 2 17 8 2019 6 nil 0)`, whereas `(decode-time time t 'integer)` returns `(31 39 2 17 8 2019 6 nil 0)`. If `form` is omitted or `nil`, it currently defaults to `integer` but this default may change in future Emacs releases, so callers requiring a particular form should specify `form`.

    **Common Lisp Note:** Common Lisp has different meanings for `dow` and `utcoff`, and its `second` is an integer between 0 and 59 inclusive.

    To access (or alter) the elements in the time value, the `decoded-time-second`, `decoded-time-minute`, `decoded-time-hour`, `decoded-time-day`, `decoded-time-month`, `decoded-time-year`, `decoded-time-weekday`, `decoded-time-dst` and `decoded-time-zone` accessors can be used.

    For instance, to increase the year in a decoded time, you could say:

    ```lisp
    (setf (decoded-time-year decoded-time)
          (+ (decoded-time-year decoded-time) 4))
    ```

    Also see the following function.

<!---->

*   Function: **decoded-time-add** *time delta*

    This function takes a decoded time structure and adds `delta` (also a decoded time structure) to it. Elements in `delta` that are `nil` are ignored.

    For instance, if you want “same time next month”, you could say:

    ```lisp
    (let ((time (decode-time nil nil t))
          (delta (make-decoded-time :month 2)))
       (encode-time (decoded-time-add time delta)))
    ```

    If this date doesn’t exist (if you’re running this on January 31st, for instance), then the date will be shifted back until you get a valid date (which will be February 28th or 29th, depending).

    Fields are added in a most to least significant order, so if the adjustment described above happens, it happens before adding days, hours, minutes or seconds.

    The values in `delta` can be negative to subtract values instead.

    The return value is a decoded time structure.

<!---->

*   Function: **make-decoded-time** *\&key second minute hour day month year dst zone*

    Return a decoded time structure with only the given keywords filled out, leaving the rest `nil`. For instance, to get a structure that represents “two months”, you could say:

    ```lisp
    (make-decoded-time :month 2)
    ```

<!---->

*   Function: **encode-time** *time \&rest obsolescent-arguments*

    This function converts `time` to a Lisp timestamp. It can act as the inverse of `decode-time`.

    Ordinarily the first argument is a list `(second minute hour day month year ignored dst zone)` that specifies a decoded time in the style of `decode-time`, so that `(encode-time (decode-time ...))` works. For the meanings of these list members, see the table under `decode-time`.

    As an obsolescent calling convention, this function can be given six or more arguments. The first six arguments `second`, `minute`, `hour`, `day`, `month`, and `year` specify most of the components of a decoded time. If there are more than six arguments the *last* argument is used as `zone` and any other extra arguments are ignored, so that `(apply #'encode-time (decode-time ...))` works. In this obsolescent convention, `zone` defaults to the current time zone rule (see [Time Zone Rules](Time-Zone-Rules.html)), and `dst` is treated as if it was -1.

    Year numbers less than 100 are not treated specially. If you want them to stand for years above 1900, or years above 2000, you must alter them yourself before you call `encode-time`. The operating system limits the range of time and zone values.

    The `encode-time` function acts as a rough inverse to `decode-time`. For example, you can pass the output of the latter to the former as follows:

    ```lisp
    (encode-time (decode-time …))
    ```

    You can perform simple date arithmetic by using out-of-range values for `seconds`, `minutes`, `hour`, `day`, and `month`; for example, day 0 means the day preceding the given month.

***

#### Footnotes

##### [(25)](#DOCF25)

Currently a positive integer `form` should be at least 65536 if the returned value is intended to be given to standard functions expecting Lisp timestamps.

Next: [Time Parsing](Time-Parsing.html), Previous: [Time Zone Rules](Time-Zone-Rules.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
