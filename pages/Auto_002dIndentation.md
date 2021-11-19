

Next: [Desktop Save Mode](Desktop-Save-Mode.html), Previous: [Font Lock Mode](Font-Lock-Mode.html), Up: [Modes](Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 23.7 Automatic Indentation of code

For programming languages, an important feature of a major mode is to provide automatic indentation. There are two parts: one is to decide what is the right indentation of a line, and the other is to decide when to reindent a line. By default, Emacs reindents a line whenever you type a character in `electric-indent-chars`, which by default only includes Newline. Major modes can add chars to `electric-indent-chars` according to the syntax of the language.

Deciding what is the right indentation is controlled in Emacs by `indent-line-function` (see [Mode-Specific Indent](Mode_002dSpecific-Indent.html)). For some modes, the *right* indentation cannot be known reliably, typically because indentation is significant so several indentations are valid but with different meanings. In that case, the mode should set `electric-indent-inhibit` to make sure the line is not constantly re-indented against the user’s wishes.

Writing a good indentation function can be difficult and to a large extent it is still a black art. Many major mode authors will start by writing a simple indentation function that works for simple cases, for example by comparing with the indentation of the previous text line. For most programming languages that are not really line-based, this tends to scale very poorly: improving such a function to let it handle more diverse situations tends to become more and more difficult, resulting in the end with a large, complex, unmaintainable indentation function which nobody dares to touch.

A good indentation function will usually need to actually parse the text, according to the syntax of the language. Luckily, it is not necessary to parse the text in as much detail as would be needed for a compiler, but on the other hand, the parser embedded in the indentation code will want to be somewhat friendly to syntactically incorrect code.

Good maintainable indentation functions usually fall into two categories: either parsing forward from some safe starting point until the position of interest, or parsing backward from the position of interest. Neither of the two is a clearly better choice than the other: parsing backward is often more difficult than parsing forward because programming languages are designed to be parsed forward, but for the purpose of indentation it has the advantage of not needing to guess a safe starting point, and it generally enjoys the property that only a minimum of text will be analyzed to decide the indentation of a line, so indentation will tend to be less affected by syntax errors in some earlier unrelated piece of code. Parsing forward on the other hand is usually easier and has the advantage of making it possible to reindent efficiently a whole region at a time, with a single parse.

Rather than write your own indentation function from scratch, it is often preferable to try and reuse some existing ones or to rely on a generic indentation engine. There are sadly few such engines. The CC-mode indentation code (used with C, C++, Java, Awk and a few other such modes) has been made more generic over the years, so if your language seems somewhat similar to one of those languages, you might try to use that engine. Another one is SMIE which takes an approach in the spirit of Lisp sexps and adapts it to non-Lisp languages.

|                     |    |                                     |
| :------------------ | -- | :---------------------------------- |
| • [SMIE](SMIE.html) |    | A simple minded indentation engine. |

Next: [Desktop Save Mode](Desktop-Save-Mode.html), Previous: [Font Lock Mode](Font-Lock-Mode.html), Up: [Modes](Modes.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
