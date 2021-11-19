

Next: [Loading Non-ASCII](Loading-Non_002dASCII.html), Previous: [Load Suffixes](Load-Suffixes.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 16.3 Library Search

When Emacs loads a Lisp library, it searches for the library in a list of directories specified by the variable `load-path`.

*   Variable: **load-path**

    The value of this variable is a list of directories to search when loading files with `load`. Each element is a string (which must be a directory) or `nil` (which stands for the current working directory).

When Emacs starts up, it sets up the value of `load-path` in several steps. First, it initializes `load-path` using default locations set when Emacs was compiled. Normally, this is a directory something like

```lisp
"/usr/local/share/emacs/version/lisp"
```

(In this and the following examples, replace `/usr/local` with the installation prefix appropriate for your Emacs.) These directories contain the standard Lisp files that come with Emacs. If Emacs cannot find them, it will not start correctly.

If you run Emacs from the directory where it was built—that is, an executable that has not been formally installed—Emacs instead initializes `load-path` using the `lisp` directory in the directory containing the sources from which it was built. If you built Emacs in a separate directory from the sources, it also adds the lisp directories from the build directory. (In all cases, elements are represented as absolute file names.)

Unless you start Emacs with the `--no-site-lisp` option, it then adds two more `site-lisp` directories to the front of `load-path`. These are intended for locally installed Lisp files, and are normally of the form:

```lisp
"/usr/local/share/emacs/version/site-lisp"
```

and

```lisp
"/usr/local/share/emacs/site-lisp"
```

The first one is for locally installed files for a specific Emacs version; the second is for locally installed files meant for use with all installed Emacs versions. (If Emacs is running uninstalled, it also adds `site-lisp` directories from the source and build directories, if they exist. Normally these directories do not contain `site-lisp` directories.)

If the environment variable `EMACSLOADPATH` is set, it modifies the above initialization procedure. Emacs initializes `load-path` based on the value of the environment variable.

The syntax of `EMACSLOADPATH` is the same as used for `PATH`; directories are separated by ‘`:`’ (or ‘`;`’, on some operating systems). Here is an example of how to set `EMACSLOADPATH` variable (from a `sh`-style shell):

```lisp
export EMACSLOADPATH=/home/foo/.emacs.d/lisp:
```

An empty element in the value of the environment variable, whether trailing (as in the above example), leading, or embedded, is replaced by the default value of `load-path` as determined by the standard initialization procedure. If there are no such empty elements, then `EMACSLOADPATH` specifies the entire `load-path`. You must include either an empty element, or the explicit path to the directory containing the standard Lisp files, else Emacs will not function. (Another way to modify `load-path` is to use the `-L` command-line option when starting Emacs; see below.)

For each directory in `load-path`, Emacs then checks to see if it contains a file `subdirs.el`, and if so, loads it. The `subdirs.el` file is created when Emacs is built/installed, and contains code that causes Emacs to add any subdirectories of those directories to `load-path`. Both immediate subdirectories and subdirectories multiple levels down are added. But it excludes subdirectories whose names do not start with a letter or digit, and subdirectories named `RCS` or `CVS`, and subdirectories containing a file named `.nosearch`.

Next, Emacs adds any extra load directories that you specify using the `-L` command-line option (see [Action Arguments](https://www.gnu.org/software/emacs/manual/html_node/emacs/Action-Arguments.html#Action-Arguments) in The GNU Emacs Manual). It also adds the directories where optional packages are installed, if any (see [Packaging Basics](Packaging-Basics.html)).

It is common to add code to one’s init file (see [Init File](Init-File.html)) to add one or more directories to `load-path`. For example:

```lisp
(push "~/.emacs.d/lisp" load-path)
```

Dumping Emacs uses a special value of `load-path`. If you use a `site-load.el` or `site-init.el` file to customize the dumped Emacs (see [Building Emacs](Building-Emacs.html)), any changes to `load-path` that these files make will be lost after dumping.

*   Command: **locate-library** *library \&optional nosuffix path interactive-call*

    This command finds the precise file name for library `library`. It searches for the library in the same way `load` does, and the argument `nosuffix` has the same meaning as in `load`: don’t add suffixes ‘`.elc`’ or ‘`.el`’ to the specified name `library`.

    If the `path` is non-`nil`, that list of directories is used instead of `load-path`.

    When `locate-library` is called from a program, it returns the file name as a string. When the user runs `locate-library` interactively, the argument `interactive-call` is `t`, and this tells `locate-library` to display the file name in the echo area.

<!---->

*   Command: **list-load-path-shadows** *\&optional stringp*

    This command shows a list of *shadowed* Emacs Lisp files. A shadowed file is one that will not normally be loaded, despite being in a directory on `load-path`, due to the existence of another similarly-named file in a directory earlier on `load-path`.

    For instance, suppose `load-path` is set to

    ```lisp
      ("/opt/emacs/site-lisp" "/usr/share/emacs/23.3/lisp")
    ```

    and that both these directories contain a file named `foo.el`. Then `(require 'foo)` never loads the file in the second directory. Such a situation might indicate a problem in the way Emacs was installed.

    When called from Lisp, this function prints a message listing the shadowed files, instead of displaying them in a buffer. If the optional argument `stringp` is non-`nil`, it instead returns the shadowed files as a string.

Next: [Loading Non-ASCII](Loading-Non_002dASCII.html), Previous: [Load Suffixes](Load-Suffixes.html), Up: [Loading](Loading.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
