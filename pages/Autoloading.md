

Previous: [Special Forms](Special-Forms.html), Up: [Forms](Forms.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 10.2.8 Autoloading

The *autoload* feature allows you to call a function or macro whose function definition has not yet been loaded into Emacs. It specifies which file contains the definition. When an autoload object appears as a symbol’s function definition, calling that symbol as a function automatically loads the specified file; then it calls the real definition loaded from that file. The way to arrange for an autoload object to appear as a symbol’s function definition is described in [Autoload](Autoload.html).
