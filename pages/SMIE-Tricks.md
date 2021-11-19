

Next: [SMIE Indentation](SMIE-Indentation.html), Previous: [SMIE Lexer](SMIE-Lexer.html), Up: [SMIE](SMIE.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 23.7.1.5 Living With a Weak Parser

The parsing technique used by SMIE does not allow tokens to behave differently in different contexts. For most programming languages, this manifests itself by precedence conflicts when converting the BNF grammar.

Sometimes, those conflicts can be worked around by expressing the grammar slightly differently. For example, for Modula-2 it might seem natural to have a BNF grammar that looks like this:

```lisp
  ...
  (inst ("IF" exp "THEN" insts "ELSE" insts "END")
        ("CASE" exp "OF" cases "END")
        ...)
  (cases (cases "|" cases)
         (caselabel ":" insts)
         ("ELSE" insts))
  ...
```

But this will create conflicts for `"ELSE"`: on the one hand, the IF rule implies (among many other things) that `"ELSE" = "END"`; but on the other hand, since `"ELSE"` appears within `cases`, which appears left of `"END"`, we also have `"ELSE" > "END"`. We can solve the conflict either by using:

```lisp
  ...
  (inst ("IF" exp "THEN" insts "ELSE" insts "END")
        ("CASE" exp "OF" cases "END")
        ("CASE" exp "OF" cases "ELSE" insts "END")
        ...)
  (cases (cases "|" cases) (caselabel ":" insts))
  ...
```

or

```lisp
  ...
  (inst ("IF" exp "THEN" else "END")
        ("CASE" exp "OF" cases "END")
        ...)
  (else (insts "ELSE" insts))
  (cases (cases "|" cases) (caselabel ":" insts) (else))
  ...
```

Reworking the grammar to try and solve conflicts has its downsides, tho, because SMIE assumes that the grammar reflects the logical structure of the code, so it is preferable to keep the BNF closer to the intended abstract syntax tree.

Other times, after careful consideration you may conclude that those conflicts are not serious and simply resolve them via the `resolvers` argument of `smie-bnf->prec2`. Usually this is because the grammar is simply ambiguous: the conflict does not affect the set of programs described by the grammar, but only the way those programs are parsed. This is typically the case for separators and associative infix operators, where you want to add a resolver like `'((assoc "|"))`. Another case where this can happen is for the classic *dangling else* problem, where you will use `'((assoc "else" "then"))`. It can also happen for cases where the conflict is real and cannot really be resolved, but it is unlikely to pose a problem in practice.

Finally, in many cases some conflicts will remain despite all efforts to restructure the grammar. Do not despair: while the parser cannot be made more clever, you can make the lexer as smart as you want. So, the solution is then to look at the tokens involved in the conflict and to split one of those tokens into 2 (or more) different tokens. E.g., if the grammar needs to distinguish between two incompatible uses of the token `"begin"`, make the lexer return different tokens (say `"begin-fun"` and `"begin-plain"`) depending on which kind of `"begin"` it finds. This pushes the work of distinguishing the different cases to the lexer, which will thus have to look at the surrounding text to find ad-hoc clues.

Next: [SMIE Indentation](SMIE-Indentation.html), Previous: [SMIE Lexer](SMIE-Lexer.html), Up: [SMIE](SMIE.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
