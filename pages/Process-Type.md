

Next: [Thread Type](Thread-Type.html), Previous: [Frame Configuration Type](Frame-Configuration-Type.html), Up: [Editing Types](Editing-Types.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 2.5.8 Process Type

The word *process* usually means a running program. Emacs itself runs in a process of this sort. However, in Emacs Lisp, a process is a Lisp object that designates a subprocess created by the Emacs process. Programs such as shells, GDB, ftp, and compilers, running in subprocesses of Emacs, extend the capabilities of Emacs. An Emacs subprocess takes textual input from Emacs and returns textual output to Emacs for further manipulation. Emacs can also send signals to the subprocess.

Process objects have no read syntax. They print in hash notation, giving the name of the process:

```lisp
(process-list)
     ⇒ (#<process shell>)
```

See [Processes](Processes.html), for information about functions that create, delete, return information about, send input or signals to, and receive output from processes.
