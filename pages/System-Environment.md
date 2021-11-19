

Next: [User Identification](User-Identification.html), Previous: [Getting Out](Getting-Out.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.3 Operating System Environment

Emacs provides access to variables in the operating system environment through various functions. These variables include the name of the system, the user’s UID, and so on.

*   Variable: **system-configuration**

    This variable holds the standard GNU configuration name for the hardware/software configuration of your system, as a string. For example, a typical value for a 64-bit GNU/Linux system is ‘`"x86_64-unknown-linux-gnu"`’.

<!---->

*   Variable: **system-type**

    The value of this variable is a symbol indicating the type of operating system Emacs is running on. The possible values are:

    *   `aix`

        IBM’s AIX.

    *   `berkeley-unix`

        Berkeley BSD and its variants.

    *   `cygwin`

        Cygwin, a POSIX layer on top of MS-Windows.

    *   `darwin`

        Darwin (macOS).

    *   `gnu`

        The GNU system (using the GNU kernel, which consists of the HURD and Mach).

    *   `gnu/linux`

        A GNU/Linux system—that is, a variant GNU system, using the Linux kernel. (These systems are the ones people often call “Linux”, but actually Linux is just the kernel, not the whole system.)

    *   `gnu/kfreebsd`

        A GNU (glibc-based) system with a FreeBSD kernel.

    *   `hpux`

        Hewlett-Packard HPUX operating system.

    *   `nacl`

        Google Native Client (NaCl) sandboxing system.

    *   `ms-dos`

        Microsoft’s DOS. Emacs compiled with DJGPP for MS-DOS binds `system-type` to `ms-dos` even when you run it on MS-Windows.

    *   `usg-unix-v`

        AT\&T Unix System V.

    *   `windows-nt`

        Microsoft Windows NT, 9X and later. The value of `system-type` is always `windows-nt`, e.g., even on Windows 10.

    We do not wish to add new symbols to make finer distinctions unless it is absolutely necessary! In fact, we hope to eliminate some of these alternatives in the future. If you need to make a finer distinction than `system-type` allows for, you can test `system-configuration`, e.g., against a regexp.

<!---->

*   Function: **system-name**

    This function returns the name of the machine you are running on, as a string.

<!---->

*   User Option: **mail-host-address**

    If this variable is non-`nil`, it is used instead of `system-name` for purposes of generating email addresses. For example, it is used when constructing the default value of `user-mail-address`. See [User Identification](User-Identification.html).

<!---->

*   Command: **getenv** *var \&optional frame*

    This function returns the value of the environment variable `var`, as a string. `var` should be a string. If `var` is undefined in the environment, `getenv` returns `nil`. It returns ‘`""`’ if `var` is set but null. Within Emacs, a list of environment variables and their values is kept in the variable `process-environment`.

    ```lisp
    (getenv "USER")
         ⇒ "lewis"
    ```

    The shell command `printenv` prints all or part of the environment:

    ```lisp
    bash$ printenv
    PATH=/usr/local/bin:/usr/bin:/bin
    USER=lewis
    ```

    ```lisp
    TERM=xterm
    SHELL=/bin/bash
    HOME=/home/lewis
    ```

    ```lisp
    …
    ```

<!---->

*   Command: **setenv** *variable \&optional value substitute*

    This command sets the value of the environment variable named `variable` to `value`. `variable` should be a string. Internally, Emacs Lisp can handle any string. However, normally `variable` should be a valid shell identifier, that is, a sequence of letters, digits and underscores, starting with a letter or underscore. Otherwise, errors may occur if subprocesses of Emacs try to access the value of `variable`. If `value` is omitted or `nil` (or, interactively, with a prefix argument), `setenv` removes `variable` from the environment. Otherwise, `value` should be a string.

    If the optional argument `substitute` is non-`nil`, Emacs calls the function `substitute-env-vars` to expand any environment variables in `value`.

    `setenv` works by modifying `process-environment`; binding that variable with `let` is also reasonable practice.

    `setenv` returns the new value of `variable`, or `nil` if it removed `variable` from the environment.

<!---->

*   Variable: **process-environment**

    This variable is a list of strings, each describing one environment variable. The functions `getenv` and `setenv` work by means of this variable.

    ```lisp
    process-environment
    ⇒ ("PATH=/usr/local/bin:/usr/bin:/bin"
        "USER=lewis"
    ```

    ```lisp
        "TERM=xterm"
        "SHELL=/bin/bash"
        "HOME=/home/lewis"
        …)
    ```

    If `process-environment` contains multiple elements that specify the same environment variable, the first of these elements specifies the variable, and the others are ignored.

<!---->

*   Variable: **initial-environment**

    This variable holds the list of environment variables Emacs inherited from its parent process when Emacs started.

<!---->

*   Variable: **path-separator**

    This variable holds a string that says which character separates directories in a search path (as found in an environment variable). Its value is `":"` for Unix and GNU systems, and `";"` for MS systems.

<!---->

*   Function: **parse-colon-path** *path*

    This function takes a search path string such as the value of the `PATH` environment variable, and splits it at the separators, returning a list of directories. `nil` in this list means the current directory. Although the function’s name says “colon”, it actually uses the value of `path-separator`.

    ```lisp
    (parse-colon-path ":/foo:/bar")
         ⇒ (nil "/foo/" "/bar/")
    ```

<!---->

*   Variable: **invocation-name**

    This variable holds the program name under which Emacs was invoked. The value is a string, and does not include a directory name.

<!---->

*   Variable: **invocation-directory**

    This variable holds the directory in which the Emacs executable was located when it was run, or `nil` if that directory cannot be determined.

<!---->

*   Variable: **installation-directory**

    If non-`nil`, this is a directory within which to look for the `lib-src` and `etc` subdirectories. In an installed Emacs, it is normally `nil`. It is non-`nil` when Emacs can’t find those directories in their standard installed locations, but can find them in a directory related somehow to the one containing the Emacs executable (i.e., `invocation-directory`).

<!---->

*   Function: **load-average** *\&optional use-float*

    This function returns the current 1-minute, 5-minute, and 15-minute system load averages, in a list. The load average indicates the number of processes trying to run on the system.

    By default, the values are integers that are 100 times the system load averages, but if `use-float` is non-`nil`, then they are returned as floating-point numbers without multiplying by 100.

    If it is impossible to obtain the load average, this function signals an error. On some platforms, access to load averages requires installing Emacs as setuid or setgid so that it can read kernel information, and that usually isn’t advisable.

    If the 1-minute load average is available, but the 5- or 15-minute averages are not, this function returns a shortened list containing the available averages.

    ```lisp
    (load-average)
         ⇒ (169 48 36)
    ```

    ```lisp
    (load-average t)
         ⇒ (1.69 0.48 0.36)
    ```

    The shell command `uptime` returns similar information.

<!---->

*   Function: **emacs-pid**

    This function returns the process ID of the Emacs process, as an integer.

<!---->

*   Variable: **tty-erase-char**

    This variable holds the erase character that was selected in the system’s terminal driver, before Emacs was started.

Next: [User Identification](User-Identification.html), Previous: [Getting Out](Getting-Out.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
