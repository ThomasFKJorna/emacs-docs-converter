

Next: [Docs and Compilation](Docs-and-Compilation.html), Previous: [Speed of Byte-Code](Speed-of-Byte_002dCode.html), Up: [Byte Compilation](Byte-Compilation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 17.2 Byte-Compilation Functions

You can byte-compile an individual function or macro definition with the `byte-compile` function. You can compile a whole file with `byte-compile-file`, or several files with `byte-recompile-directory` or `batch-byte-compile`.

Sometimes, the byte compiler produces warning and/or error messages (see [Compiler Errors](Compiler-Errors.html), for details). These messages are normally recorded in a buffer called `*Compile-Log*`, which uses Compilation mode. See [Compilation Mode](https://www.gnu.org/software/emacs/manual/html_node/emacs/Compilation-Mode.html#Compilation-Mode) in The GNU Emacs Manual. However, if the variable `byte-compile-debug` is non-`nil`, error messages will be signaled as Lisp errors instead (see [Errors](Errors.html)).

Be careful when writing macro calls in files that you intend to byte-compile. Since macro calls are expanded when they are compiled, the macros need to be loaded into Emacs or the byte compiler will not do the right thing. The usual way to handle this is with `require` forms which specify the files containing the needed macro definitions (see [Named Features](Named-Features.html)). Normally, the byte compiler does not evaluate the code that it is compiling, but it handles `require` forms specially, by loading the specified libraries. To avoid loading the macro definition files when someone *runs* the compiled program, write `eval-when-compile` around the `require` calls (see [Eval During Compile](Eval-During-Compile.html)). For more details, See [Compiling Macros](Compiling-Macros.html).

Inline (`defsubst`) functions are less troublesome; if you compile a call to such a function before its definition is known, the call will still work right, it will just run slower.

*   Function: **byte-compile** *symbol*

    This function byte-compiles the function definition of `symbol`, replacing the previous definition with the compiled one. The function definition of `symbol` must be the actual code for the function; `byte-compile` does not handle function indirection. The return value is the byte-code function object which is the compiled definition of `symbol` (see [Byte-Code Objects](Byte_002dCode-Objects.html)).

    ```lisp
    (defun factorial (integer)
      "Compute factorial of INTEGER."
      (if (= 1 integer) 1
        (* integer (factorial (1- integer)))))
    ⇒ factorial
    ```

    ```lisp
    ```

    ```lisp
    (byte-compile 'factorial)
    ⇒
    #[(integer)
      "^H\301U\203^H^@\301\207\302^H\303^HS!\"\207"
      [integer 1 * factorial]
      4 "Compute factorial of INTEGER."]
    ```

    If `symbol`’s definition is a byte-code function object, `byte-compile` does nothing and returns `nil`. It does not compile the symbol’s definition again, since the original (non-compiled) code has already been replaced in the symbol’s function cell by the byte-compiled code.

    The argument to `byte-compile` can also be a `lambda` expression. In that case, the function returns the corresponding compiled code but does not store it anywhere.

<!---->

*   Command: **compile-defun** *\&optional arg*

    This command reads the defun containing point, compiles it, and evaluates the result. If you use this on a defun that is actually a function definition, the effect is to install a compiled version of that function.

    `compile-defun` normally displays the result of evaluation in the echo area, but if `arg` is non-`nil`, it inserts the result in the current buffer after the form it has compiled.

<!---->

*   Command: **byte-compile-file** *filename \&optional load*

    This function compiles a file of Lisp code named `filename` into a file of byte-code. The output file’s name is made by changing the ‘`.el`’ suffix into ‘`.elc`’; if `filename` does not end in ‘`.el`’, it adds ‘`.elc`’ to the end of `filename`.

    Compilation works by reading the input file one form at a time. If it is a definition of a function or macro, the compiled function or macro definition is written out. Other forms are batched together, then each batch is compiled, and written so that its compiled code will be executed when the file is read. All comments are discarded when the input file is read.

    This command returns `t` if there were no errors and `nil` otherwise. When called interactively, it prompts for the file name.

    If `load` is non-`nil`, this command loads the compiled file after compiling it. Interactively, `load` is the prefix argument.

    ```lisp
    $ ls -l push*
    -rw-r--r-- 1 lewis lewis 791 Oct  5 20:31 push.el
    ```

    ```lisp
    ```

    ```lisp
    (byte-compile-file "~/emacs/push.el")
         ⇒ t
    ```

    ```lisp
    ```

    ```lisp
    $ ls -l push*
    -rw-r--r-- 1 lewis lewis 791 Oct  5 20:31 push.el
    -rw-rw-rw- 1 lewis lewis 638 Oct  8 20:25 push.elc
    ```

<!---->

*   Command: **byte-recompile-directory** *directory \&optional flag force*

    This command recompiles every ‘`.el`’ file in `directory` (or its subdirectories) that needs recompilation. A file needs recompilation if a ‘`.elc`’ file exists but is older than the ‘`.el`’ file.

    When a ‘`.el`’ file has no corresponding ‘`.elc`’ file, `flag` says what to do. If it is `nil`, this command ignores these files. If `flag` is 0, it compiles them. If it is neither `nil` nor 0, it asks the user whether to compile each such file, and asks about each subdirectory as well.

    Interactively, `byte-recompile-directory` prompts for `directory` and `flag` is the prefix argument.

    If `force` is non-`nil`, this command recompiles every ‘`.el`’ file that has a ‘`.elc`’ file.

    The returned value is unpredictable.

<!---->

*   Function: **batch-byte-compile** *\&optional noforce*

    This function runs `byte-compile-file` on files specified on the command line. This function must be used only in a batch execution of Emacs, as it kills Emacs on completion. An error in one file does not prevent processing of subsequent files, but no output file will be generated for it, and the Emacs process will terminate with a nonzero status code.

    If `noforce` is non-`nil`, this function does not recompile files that have an up-to-date ‘`.elc`’ file.

    ```lisp
    $ emacs -batch -f batch-byte-compile *.el
    ```

Next: [Docs and Compilation](Docs-and-Compilation.html), Previous: [Speed of Byte-Code](Speed-of-Byte_002dCode.html), Up: [Byte Compilation](Byte-Compilation.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
