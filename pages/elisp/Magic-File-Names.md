

### 25.12 Making Certain File Names “Magic”

You can implement special handling for certain file names. This is called making those names *magic*. The principal use for this feature is in implementing access to remote files (see [Remote Files](https://www.gnu.org/software/emacs/manual/html_node/emacs/Remote-Files.html#Remote-Files) in The GNU Emacs Manual).

To define a kind of magic file name, you must supply a regular expression to define the class of names (all those that match the regular expression), plus a handler that implements all the primitive Emacs file operations for file names that match.

The variable `file-name-handler-alist` holds a list of handlers, together with regular expressions that determine when to apply each handler. Each element has this form:

```lisp
(regexp . handler)
```

All the Emacs primitives for file access and file name transformation check the given file name against `file-name-handler-alist`. If the file name matches `regexp`, the primitives handle that file by calling `handler`.

The first argument given to `handler` is the name of the primitive, as a symbol; the remaining arguments are the arguments that were passed to that primitive. (The first of these arguments is most often the file name itself.) For example, if you do this:

```lisp
(file-exists-p filename)
```

and `filename` has handler `handler`, then `handler` is called like this:

```lisp
(funcall handler 'file-exists-p filename)
```

When a function takes two or more arguments that must be file names, it checks each of those names for a handler. For example, if you do this:

```lisp
(expand-file-name filename dirname)
```

then it checks for a handler for `filename` and then for a handler for `dirname`. In either case, the `handler` is called like this:

```lisp
(funcall handler 'expand-file-name filename dirname)
```

The `handler` then needs to figure out whether to handle `filename` or `dirname`.

If the specified file name matches more than one handler, the one whose match starts last in the file name gets precedence. This rule is chosen so that handlers for jobs such as uncompression are handled first, before handlers for jobs such as remote file access.

Here are the operations that a magic file name handler gets to handle:

`access-file`, `add-name-to-file`, `byte-compiler-base-file-name`,\
`copy-directory`, `copy-file`, `delete-directory`, `delete-file`, `diff-latest-backup-file`, `directory-file-name`, `directory-files`, `directory-files-and-attributes`, `dired-compress-file`, `dired-uncache`, `exec-path`, `expand-file-name`,\
`file-accessible-directory-p`, `file-acl`, `file-attributes`, `file-directory-p`, `file-equal-p`, `file-executable-p`, `file-exists-p`, `file-in-directory-p`, `file-local-copy`, `file-modes`, `file-name-all-completions`, `file-name-as-directory`, `file-name-case-insensitive-p`, `file-name-completion`, `file-name-directory`, `file-name-nondirectory`, `file-name-sans-versions`, `file-newer-than-file-p`, `file-notify-add-watch`, `file-notify-rm-watch`, `file-notify-valid-p`, `file-ownership-preserved-p`, `file-readable-p`, `file-regular-p`, `file-remote-p`, `file-selinux-context`, `file-symlink-p`, `file-system-info`, `file-truename`, `file-writable-p`, `find-backup-file-name`,\
`get-file-buffer`, `insert-directory`, `insert-file-contents`,\
`load`, `make-auto-save-file-name`, `make-directory`, `make-directory-internal`, `make-nearby-temp-file`, `make-process`, `make-symbolic-link`,\
`process-file`, `rename-file`, `set-file-acl`, `set-file-modes`, `set-file-selinux-context`, `set-file-times`, `set-visited-file-modtime`, `shell-command`, `start-file-process`, `substitute-in-file-name`,\
`temporary-file-directory`, `unhandled-file-name-directory`, `vc-registered`, `verify-visited-file-modtime`,\
`write-region`.

Handlers for `insert-file-contents` typically need to clear the buffer’s modified flag, with `(set-buffer-modified-p nil)`, if the `visit` argument is non-`nil`. This also has the effect of unlocking the buffer if it is locked.

The handler function must handle all of the above operations, and possibly others to be added in the future. It need not implement all these operations itself—when it has nothing special to do for a certain operation, it can reinvoke the primitive, to handle the operation in the usual way. It should always reinvoke the primitive for an operation it does not recognize. Here’s one way to do this:

```lisp
(defun my-file-handler (operation &rest args)
  ;; First check for the specific operations
  ;; that we have special handling for.
  (cond ((eq operation 'insert-file-contents) …)
        ((eq operation 'write-region) …)
        …
        ;; Handle any operation we don’t know about.
        (t (let ((inhibit-file-name-handlers
                  (cons 'my-file-handler
                        (and (eq inhibit-file-name-operation operation)
                             inhibit-file-name-handlers)))
                 (inhibit-file-name-operation operation))
             (apply operation args)))))
```

When a handler function decides to call the ordinary Emacs primitive for the operation at hand, it needs to prevent the primitive from calling the same handler once again, thus leading to an infinite recursion. The example above shows how to do this, with the variables `inhibit-file-name-handlers` and `inhibit-file-name-operation`. Be careful to use them exactly as shown above; the details are crucial for proper behavior in the case of multiple handlers, and for operations that have two file names that may each have handlers.

Handlers that don’t really do anything special for actual access to the file—such as the ones that implement completion of host names for remote file names—should have a non-`nil` `safe-magic` property. For instance, Emacs normally protects directory names it finds in `PATH` from becoming magic, if they look like magic file names, by prefixing them with ‘`/:`’. But if the handler that would be used for them has a non-`nil` `safe-magic` property, the ‘`/:`’ is not added.

A file name handler can have an `operations` property to declare which operations it handles in a nontrivial way. If this property has a non-`nil` value, it should be a list of operations; then only those operations will call the handler. This avoids inefficiency, but its main purpose is for autoloaded handler functions, so that they won’t be loaded except when they have real work to do.

Simply deferring all operations to the usual primitives does not work. For instance, if the file name handler applies to `file-exists-p`, then it must handle `load` itself, because the usual `load` code won’t work properly in that case. However, if the handler uses the `operations` property to say it doesn’t handle `file-exists-p`, then it need not handle `load` nontrivially.

### Variable: **inhibit-file-name-handlers**

This variable holds a list of handlers whose use is presently inhibited for a certain operation.

### Variable: **inhibit-file-name-operation**

The operation for which certain handlers are presently inhibited.

### Function: **find-file-name-handler** *file operation*

This function returns the handler function for file name `file`, or `nil` if there is none. The argument `operation` should be the operation to be performed on the file—the value you will pass to the handler as its first argument when you call it. If `operation` equals `inhibit-file-name-operation`, or if it is not found in the `operations` property of the handler, this function returns `nil`.

### Function: **file-local-copy** *filename*

This function copies file `filename` to an ordinary non-magic file on the local machine, if it isn’t on the local machine already. Magic file names should handle the `file-local-copy` operation if they refer to files on other machines. A magic file name that is used for other purposes than remote file access should not handle `file-local-copy`; then this function will treat the file as local.

If `filename` is local, whether magic or not, this function does nothing and returns `nil`. Otherwise it returns the file name of the local copy file.

### Function: **file-remote-p** *filename \&optional identification connected*

This function tests whether `filename` is a remote file. If `filename` is local (not remote), the return value is `nil`. If `filename` is indeed remote, the return value is a string that identifies the remote system.

This identifier string can include a host name and a user name, as well as characters designating the method used to access the remote system. For example, the remote identifier string for the filename `/sudo::/some/file` is `/sudo:root@localhost:`.

If `file-remote-p` returns the same identifier for two different filenames, that means they are stored on the same file system and can be accessed locally with respect to each other. This means, for example, that it is possible to start a remote process accessing both files at the same time. Implementers of file name handlers need to ensure this principle is valid.

`identification` specifies which part of the identifier shall be returned as string. `identification` can be the symbol `method`, `user` or `host`; any other value is handled like `nil` and means to return the complete identifier string. In the example above, the remote `user` identifier string would be `root`.

If `connected` is non-`nil`, this function returns `nil` even if `filename` is remote, if Emacs has no network connection to its host. This is useful when you want to avoid the delay of making connections when they don’t exist.

### Function: **unhandled-file-name-directory** *filename*

This function returns the name of a directory that is not magic. For a non-magic `filename` it returns the corresponding directory name (see [Directory Names](Directory-Names.html)). For a magic `filename`, it invokes the file name handler, which therefore decides what value to return. If `filename` is not accessible from a local process, then the file name handler should indicate that by returning `nil`.

This is useful for running a subprocess; every subprocess must have a non-magic directory to serve as its current directory, and this function is a good way to come up with one.

### Function: **file-local-name** *filename*

This function returns the *local part* of `filename`. This is the part of the file’s name that identifies it on the remote host, and is typically obtained by removing from the remote file name the parts that specify the remote host and the method of accessing it. For example:

```lisp
(file-local-name "/ssh:user@host:/foo/bar")
     ⇒ "/foo/bar"
```

For a remote `filename`, this function returns a file name which could be used directly as an argument of a remote process (see [Asynchronous Processes](Asynchronous-Processes.html), and see [Synchronous Processes](Synchronous-Processes.html)), and as the program to run on the remote host. If `filename` is local, this function returns it unchanged.

### User Option: **remote-file-name-inhibit-cache**

The attributes of remote files can be cached for better performance. If they are changed outside of Emacs’s control, the cached values become invalid, and must be reread.

When this variable is set to `nil`, cached values are never expired. Use this setting with caution, only if you are sure nothing other than Emacs ever changes the remote files. If it is set to `t`, cached values are never used. This is the safest value, but could result in performance degradation.

A compromise is to set it to a positive number. This means that cached values are used for that amount of seconds since they were cached. If a remote file is checked regularly, it might be a good idea to let-bind this variable to a value less than the time period between consecutive checks. For example:

```lisp
(defun display-time-file-nonempty-p (file)
  (let ((remote-file-name-inhibit-cache
         (- display-time-interval 5)))
    (and (file-exists-p file)
         (< 0 (file-attribute-size
               (file-attributes
                (file-chase-links file)))))))
```
