

### 35.8 Categories

*Categories* provide an alternate way of classifying characters syntactically. You can define several categories as needed, then independently assign each character to one or more categories. Unlike syntax classes, categories are not mutually exclusive; it is normal for one character to belong to several categories.

Each buffer has a *category table* which records which categories are defined and also which characters belong to each category. Each category table defines its own categories, but normally these are initialized by copying from the standard categories table, so that the standard categories are available in all modes.

Each category has a name, which is an ASCII printing character in the range ‘` `’ to ‘`~`’. You specify the name of a category when you define it with `define-category`.

The category table is actually a char-table (see [Char-Tables](Char_002dTables.html)). The element of the category table at index `c` is a *category set*—a bool-vector—that indicates which categories character `c` belongs to. In this category set, if the element at index `cat` is `t`, that means category `cat` is a member of the set, and that character `c` belongs to category `cat`.

For the next three functions, the optional argument `table` defaults to the current buffer’s category table.

### Function: **define-category** *char docstring \&optional table*

This function defines a new category, with name `char` and documentation `docstring`, for the category table `table`.

Here’s an example of defining a new category for characters that have strong right-to-left directionality (see [Bidirectional Display](Bidirectional-Display.html)) and using it in a special category table. To obtain the information about the directionality of characters, the example code uses the ‘`bidi-class`’ Unicode property (see [bidi-class](Character-Properties.html)).

```lisp
(defvar special-category-table-for-bidi
  ;;     Make an empty category-table.
  (let ((category-table (make-category-table))
        ;; Create a char-table which gives the 'bidi-class' Unicode
        ;; property for each character.
        (uniprop-table
         (unicode-property-table-internal 'bidi-class)))
    (define-category ?R "Characters of bidi-class R, AL, or RLO"
                     category-table)
    ;; Modify the category entry of each character whose
    ;; 'bidi-class' Unicode property is R, AL, or RLO --
    ;; these have a right-to-left directionality.
    (map-char-table
     (lambda (key val)
       (if (memq val '(R AL RLO))
           (modify-category-entry key ?R category-table)))
     uniprop-table)
    category-table))
```

### Function: **category-docstring** *category \&optional table*

This function returns the documentation string of category `category` in category table `table`.

```lisp
(category-docstring ?a)
     ⇒ "ASCII"
(category-docstring ?l)
     ⇒ "Latin"
```

### Function: **get-unused-category** *\&optional table*

This function returns a category name (a character) which is not currently defined in `table`. If all possible categories are in use in `table`, it returns `nil`.

### Function: **category-table**

This function returns the current buffer’s category table.

### Function: **category-table-p** *object*

This function returns `t` if `object` is a category table, otherwise `nil`.

### Function: **standard-category-table**

This function returns the standard category table.

### Function: **copy-category-table** *\&optional table*

This function constructs a copy of `table` and returns it. If `table` is not supplied (or is `nil`), it returns a copy of the standard category table. Otherwise, an error is signaled if `table` is not a category table.

### Function: **set-category-table** *table*

This function makes `table` the category table for the current buffer. It returns `table`.

### Function: **make-category-table**

This creates and returns an empty category table. In an empty category table, no categories have been allocated, and no characters belong to any categories.

### Function: **make-category-set** *categories*

This function returns a new category set—a bool-vector—whose initial contents are the categories listed in the string `categories`. The elements of `categories` should be category names; the new category set has `t` for each of those categories, and `nil` for all other categories.

```lisp
(make-category-set "al")
     ⇒ #&128"\0\0\0\0\0\0\0\0\0\0\0\0\2\20\0\0"
```

### Function: **char-category-set** *char*

This function returns the category set for character `char` in the current buffer’s category table. This is the bool-vector which records which categories the character `char` belongs to. The function `char-category-set` does not allocate storage, because it returns the same bool-vector that exists in the category table.

```lisp
(char-category-set ?a)
     ⇒ #&128"\0\0\0\0\0\0\0\0\0\0\0\0\2\20\0\0"
```

### Function: **category-set-mnemonics** *category-set*

This function converts the category set `category-set` into a string containing the characters that designate the categories that are members of the set.

```lisp
(category-set-mnemonics (char-category-set ?a))
     ⇒ "al"
```

### Function: **modify-category-entry** *char category \&optional table reset*

This function modifies the category set of `char` in category table `table` (which defaults to the current buffer’s category table). `char` can be a character, or a cons cell of the form `(min . max)`; in the latter case, the function modifies the category sets of all characters in the range between `min` and `max`, inclusive.

Normally, it modifies a category set by adding `category` to it. But if `reset` is non-`nil`, then it deletes `category` instead.

### Command: **describe-categories** *\&optional buffer-or-name*

This function describes the category specifications in the current category table. It inserts the descriptions in a buffer, and then displays that buffer. If `buffer-or-name` is non-`nil`, it describes the category table of that buffer instead.
