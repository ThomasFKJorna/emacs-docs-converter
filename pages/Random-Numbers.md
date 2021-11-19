

Previous: [Math Functions](Math-Functions.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 3.10 Random Numbers

A deterministic computer program cannot generate true random numbers. For most purposes, *pseudo-random numbers* suffice. A series of pseudo-random numbers is generated in a deterministic fashion. The numbers are not truly random, but they have certain properties that mimic a random series. For example, all possible values occur equally often in a pseudo-random series.

Pseudo-random numbers are generated from a *seed value*. Starting from any given seed, the `random` function always generates the same sequence of numbers. By default, Emacs initializes the random seed at startup, in such a way that the sequence of values of `random` (with overwhelming likelihood) differs in each Emacs run.

Sometimes you want the random number sequence to be repeatable. For example, when debugging a program whose behavior depends on the random number sequence, it is helpful to get the same behavior in each program run. To make the sequence repeat, execute `(random "")`. This sets the seed to a constant value for your particular Emacs executable (though it may differ for other Emacs builds). You can use other strings to choose various seed values.

*   Function: **random** *\&optional limit*

    This function returns a pseudo-random integer. Repeated calls return a series of pseudo-random integers.

    If `limit` is a positive fixnum, the value is chosen to be nonnegative and less than `limit`. Otherwise, the value might be any fixnum, i.e., any integer from `most-negative-fixnum` through `most-positive-fixnum` (see [Integer Basics](Integer-Basics.html)).

    If `limit` is `t`, it means to choose a new seed as if Emacs were restarting, typically from the system entropy. On systems lacking entropy pools, choose the seed from less-random volatile data such as the current time.

    If `limit` is a string, it means to choose a new seed based on the string’s contents.

Previous: [Math Functions](Math-Functions.html), Up: [Numbers](Numbers.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
