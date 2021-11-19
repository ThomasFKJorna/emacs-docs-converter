

Next: [Precalculated Fontification](Precalculated-Fontification.html), Previous: [Other Font Lock Variables](Other-Font-Lock-Variables.html), Up: [Font Lock Mode](Font-Lock-Mode.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.6.5 Levels of Font Lock

Some major modes offer three different levels of fontification. You can define multiple levels by using a list of symbols for `keywords` in `font-lock-defaults`. Each symbol specifies one level of fontification; it is up to the user to choose one of these levels, normally by setting `font-lock-maximum-decoration` (see [Font Lock](https://www.gnu.org/software/emacs/manual/html_node/emacs/Font-Lock.html#Font-Lock) in the GNU Emacs Manual). The chosen level’s symbol value is used to initialize `font-lock-keywords`.

Here are the conventions for how to define the levels of fontification:

*   Level 1: highlight function declarations, file directives (such as include or import directives), strings and comments. The idea is speed, so only the most important and top-level components are fontified.
*   Level 2: in addition to level 1, highlight all language keywords, including type names that act like keywords, as well as named constant values. The idea is that all keywords (either syntactic or semantic) should be fontified appropriately.
*   Level 3: in addition to level 2, highlight the symbols being defined in function and variable declarations, and all builtin function names, wherever they appear.
