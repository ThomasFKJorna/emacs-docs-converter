

### 4.5 Comparison of Characters and Strings

### Function: **char-equal** *character1 character2*

This function returns `t` if the arguments represent the same character, `nil` otherwise. This function ignores differences in case if `case-fold-search` is non-`nil`.

```lisp
(char-equal ?x ?x)
     ⇒ t
(let ((case-fold-search nil))
  (char-equal ?x ?X))
     ⇒ nil
```

### Function: **string=** *string1 string2*

This function returns `t` if the characters of the two strings match exactly. Symbols are also allowed as arguments, in which case the symbol names are used. Case is always significant, regardless of `case-fold-search`.

This function is equivalent to `equal` for comparing two strings (see [Equality Predicates](Equality-Predicates.html)). In particular, the text properties of the two strings are ignored; use `equal-including-properties` if you need to distinguish between strings that differ only in their text properties. However, unlike `equal`, if either argument is not a string or symbol, `string=` signals an error.

```lisp
(string= "abc" "abc")
     ⇒ t
(string= "abc" "ABC")
     ⇒ nil
(string= "ab" "ABC")
     ⇒ nil
```

For technical reasons, a unibyte and a multibyte string are `equal` if and only if they contain the same sequence of character codes and all these codes are either in the range 0 through 127 (ASCII) or 160 through 255 (`eight-bit-graphic`). However, when a unibyte string is converted to a multibyte string, all characters with codes in the range 160 through 255 are converted to characters with higher codes, whereas ASCII characters remain unchanged. Thus, a unibyte string and its conversion to multibyte are only `equal` if the string is all ASCII. Character codes 160 through 255 are not entirely proper in multibyte text, even though they can occur. As a consequence, the situation where a unibyte and a multibyte string are `equal` without both being all ASCII is a technical oddity that very few Emacs Lisp programmers ever get confronted with. See [Text Representations](Text-Representations.html).

### Function: **string-equal** *string1 string2*

`string-equal` is another name for `string=`.

### Function: **string-collate-equalp** *string1 string2 \&optional locale ignore-case*

This function returns `t` if `string1` and `string2` are equal with respect to collation rules. A collation rule is not only determined by the lexicographic order of the characters contained in `string1` and `string2`, but also further rules about relations between these characters. Usually, it is defined by the `locale` environment Emacs is running with.

For example, characters with different coding points but the same meaning might be considered as equal, like different grave accent Unicode characters:

```lisp
(string-collate-equalp (string ?\uFF40) (string ?\u1FEF))
     ⇒ t
```

The optional argument `locale`, a string, overrides the setting of your current locale identifier for collation. The value is system dependent; a `locale` `"en_US.UTF-8"` is applicable on POSIX systems, while it would be, e.g., `"enu_USA.1252"` on MS-Windows systems.

If `ignore-case` is non-`nil`, characters are converted to lower-case before comparing them.

To emulate Unicode-compliant collation on MS-Windows systems, bind `w32-collate-ignore-punctuation` to a non-`nil` value, since the codeset part of the locale cannot be `"UTF-8"` on MS-Windows.

If your system does not support a locale environment, this function behaves like `string-equal`.

Do *not* use this function to compare file names for equality, as filesystems generally don’t honor linguistic equivalence of strings that collation implements.

### Function: **string\\<** *string1 string2*

This function compares two strings a character at a time. It scans both the strings at the same time to find the first pair of corresponding characters that do not match. If the lesser character of these two is the character from `string1`, then `string1` is less, and this function returns `t`. If the lesser character is the one from `string2`, then `string1` is greater, and this function returns `nil`. If the two strings match entirely, the value is `nil`.

Pairs of characters are compared according to their character codes. Keep in mind that lower case letters have higher numeric values in the ASCII character set than their upper case counterparts; digits and many punctuation characters have a lower numeric value than upper case letters. An ASCII character is less than any non-ASCII character; a unibyte non-ASCII character is always less than any multibyte non-ASCII character (see [Text Representations](Text-Representations.html)).

```lisp
(string< "abc" "abd")
     ⇒ t
(string< "abd" "abc")
     ⇒ nil
(string< "123" "abc")
     ⇒ t
```

When the strings have different lengths, and they match up to the length of `string1`, then the result is `t`. If they match up to the length of `string2`, the result is `nil`. A string of no characters is less than any other string.

```lisp
(string< "" "abc")
     ⇒ t
(string< "ab" "abc")
     ⇒ t
(string< "abc" "")
     ⇒ nil
(string< "abc" "ab")
     ⇒ nil
(string< "" "")
     ⇒ nil
```

Symbols are also allowed as arguments, in which case their print names are compared.

### Function: **string-lessp** *string1 string2*

`string-lessp` is another name for `string<`.

### Function: **string-greaterp** *string1 string2*

This function returns the result of comparing `string1` and `string2` in the opposite order, i.e., it is equivalent to calling `(string-lessp string2 string1)`.

### Function: **string-collate-lessp** *string1 string2 \&optional locale ignore-case*

This function returns `t` if `string1` is less than `string2` in collation order. A collation order is not only determined by the lexicographic order of the characters contained in `string1` and `string2`, but also further rules about relations between these characters. Usually, it is defined by the `locale` environment Emacs is running with.

For example, punctuation and whitespace characters might be ignored for sorting (see [Sequence Functions](Sequence-Functions.html)):

```lisp
(sort (list "11" "12" "1 1" "1 2" "1.1" "1.2") 'string-collate-lessp)
     ⇒ ("11" "1 1" "1.1" "12" "1 2" "1.2")
```

This behavior is system-dependent; e.g., punctuation and whitespace are never ignored on Cygwin, regardless of locale.

The optional argument `locale`, a string, overrides the setting of your current locale identifier for collation. The value is system dependent; a `locale` `"en_US.UTF-8"` is applicable on POSIX systems, while it would be, e.g., `"enu_USA.1252"` on MS-Windows systems. The `locale` value of `"POSIX"` or `"C"` lets `string-collate-lessp` behave like `string-lessp`:

```lisp
(sort (list "11" "12" "1 1" "1 2" "1.1" "1.2")
      (lambda (s1 s2) (string-collate-lessp s1 s2 "POSIX")))
     ⇒ ("1 1" "1 2" "1.1" "1.2" "11" "12")
```

If `ignore-case` is non-`nil`, characters are converted to lower-case before comparing them.

To emulate Unicode-compliant collation on MS-Windows systems, bind `w32-collate-ignore-punctuation` to a non-`nil` value, since the codeset part of the locale cannot be `"UTF-8"` on MS-Windows.

If your system does not support a locale environment, this function behaves like `string-lessp`.

### Function: **string-version-lessp** *string1 string2*

This function compares strings lexicographically, except it treats sequences of numerical characters as if they comprised a base-ten number, and then compares the numbers. So ‘`foo2.png`’ is “smaller” than ‘`foo12.png`’ according to this predicate, even if ‘`12`’ is lexicographically “smaller” than ‘`2`’.

### Function: **string-prefix-p** *string1 string2 \&optional ignore-case*

This function returns non-`nil` if `string1` is a prefix of `string2`; i.e., if `string2` starts with `string1`. If the optional argument `ignore-case` is non-`nil`, the comparison ignores case differences.

### Function: **string-suffix-p** *suffix string \&optional ignore-case*

This function returns non-`nil` if `suffix` is a suffix of `string`; i.e., if `string` ends with `suffix`. If the optional argument `ignore-case` is non-`nil`, the comparison ignores case differences.

### Function: **compare-strings** *string1 start1 end1 string2 start2 end2 \&optional ignore-case*

This function compares a specified part of `string1` with a specified part of `string2`. The specified part of `string1` runs from index `start1` (inclusive) up to index `end1` (exclusive); `nil` for `start1` means the start of the string, while `nil` for `end1` means the length of the string. Likewise, the specified part of `string2` runs from index `start2` up to index `end2`.

The strings are compared by the numeric values of their characters. For instance, `str1` is considered less than `str2` if its first differing character has a smaller numeric value. If `ignore-case` is non-`nil`, characters are converted to upper-case before comparing them. Unibyte strings are converted to multibyte for comparison (see [Text Representations](Text-Representations.html)), so that a unibyte string and its conversion to multibyte are always regarded as equal.

If the specified portions of the two strings match, the value is `t`. Otherwise, the value is an integer which indicates how many leading characters agree, and which string is less. Its absolute value is one plus the number of characters that agree at the beginning of the two strings. The sign is negative if `string1` (or its specified portion) is less.

### Function: **string-distance** *string1 string2 \&optional bytecompare*

This function returns the *Levenshtein distance* between the source string `string1` and the target string `string2`. The Levenshtein distance is the number of single-character changes—deletions, insertions, or replacements—required to transform the source string into the target string; it is one possible definition of the *edit distance* between strings.

Letter-case of the strings is significant for the computed distance, but their text properties are ignored. If the optional argument `bytecompare` is non-`nil`, the function calculates the distance in terms of bytes instead of characters. The byte-wise comparison uses the internal Emacs representation of characters, so it will produce inaccurate results for multibyte strings that include raw bytes (see [Text Representations](Text-Representations.html)); make the strings unibyte by encoding them (see [Explicit Encoding](Explicit-Encoding.html)) if you need accurate results with raw bytes.

### Function: **assoc-string** *key alist \&optional case-fold*

This function works like `assoc`, except that `key` must be a string or symbol, and comparison is done using `compare-strings`. Symbols are converted to strings before testing. If `case-fold` is non-`nil`, `key` and the elements of `alist` are converted to upper-case before comparison. Unlike `assoc`, this function can also match elements of the alist that are strings or symbols rather than conses. In particular, `alist` can be a list of strings or symbols rather than an actual alist. See [Association Lists](Association-Lists.html).

See also the function `compare-buffer-substrings` in [Comparing Text](Comparing-Text.html), for a way to compare text in buffers. The function `string-match`, which matches a regular expression against a string, can be used for a kind of string comparison; see [Regexp Search](Regexp-Search.html).
