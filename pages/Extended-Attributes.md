

Next: [Locating Files](Locating-Files.html), Previous: [File Attributes](File-Attributes.html), Up: [Information about Files](Information-about-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

#### 25.6.5 Extended File Attributes

On some operating systems, each file can be associated with arbitrary *extended file attributes*. At present, Emacs supports querying and setting two specific sets of extended file attributes: Access Control Lists (ACLs) and SELinux contexts. These extended file attributes are used, on some systems, to impose more sophisticated file access controls than the basic Unix-style permissions discussed in the previous sections.

A detailed explanation of ACLs and SELinux is beyond the scope of this manual. For our purposes, each file can be associated with an *ACL*, which specifies its properties under an ACL-based file control system, and/or an *SELinux context*, which specifies its properties under the SELinux system.

*   Function: **file-acl** *filename*

    This function returns the ACL for the file `filename`. The exact Lisp representation of the ACL is unspecified (and may change in future Emacs versions), but it is the same as what `set-file-acl` takes for its `acl` argument (see [Changing Files](Changing-Files.html)).

    The underlying ACL implementation is platform-specific; on GNU/Linux and BSD, Emacs uses the POSIX ACL interface, while on MS-Windows Emacs emulates the POSIX ACL interface with native file security APIs.

    If ACLs are not supported or the file does not exist, then the return value is `nil`.

<!---->

*   Function: **file-selinux-context** *filename*

    This function returns the SELinux context of the file `filename`, as a list of the form `(user role type range)`. The list elements are the context’s user, role, type, and range respectively, as Lisp strings; see the SELinux documentation for details about what these actually mean. The return value has the same form as what `set-file-selinux-context` takes for its `context` argument (see [Changing Files](Changing-Files.html)).

    If SELinux is not supported or the file does not exist, then the return value is `(nil nil nil nil)`.

<!---->

*   Function: **file-extended-attributes** *filename*

    This function returns an alist of the Emacs-recognized extended attributes of file `filename`. Currently, it serves as a convenient way to retrieve both the ACL and SELinux context; you can then call the function `set-file-extended-attributes`, with the returned alist as its second argument, to apply the same file access attributes to another file (see [Changing Files](Changing-Files.html)).

    One of the elements is `(acl . acl)`, where `acl` has the same form returned by `file-acl`.

    Another element is `(selinux-context . context)`, where `context` is the SELinux context, in the same form returned by `file-selinux-context`.

Next: [Locating Files](Locating-Files.html), Previous: [File Attributes](File-Attributes.html), Up: [Information about Files](Information-about-Files.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
