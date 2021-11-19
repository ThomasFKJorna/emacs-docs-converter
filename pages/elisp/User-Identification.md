<!-- This is the GNU Emacs Lisp Reference Manual
corresponding to Emacs version 27.2.

Copyright (C) 1990-1996, 1998-2021 Free Software Foundation,
Inc.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3 or
any later version published by the Free Software Foundation; with the
Invariant Sections being "GNU General Public License," with the
Front-Cover Texts being "A GNU Manual," and with the Back-Cover
Texts as in (a) below.  A copy of the license is included in the
section entitled "GNU Free Documentation License."

(a) The FSF's Back-Cover Text is: "You have the freedom to copy and
modify this GNU manual.  Buying copies from the FSF supports it in
developing GNU and promoting software freedom." -->

<!-- Created by GNU Texinfo 6.7, http://www.gnu.org/software/texinfo/ -->

Next: [Time of Day](Time-of-Day.html), Previous: [System Environment](System-Environment.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 40.4 User Identification

*   Variable: **init-file-user**

    This variable says which user’s init files should be used by Emacs—or `nil` if none. `""` stands for the user who originally logged in. The value reflects command-line options such as ‘`-q`’ or ‘`-u user`’.

    Lisp packages that load files of customizations, or any other sort of user profile, should obey this variable in deciding where to find it. They should load the profile of the user name found in this variable. If `init-file-user` is `nil`, meaning that the ‘`-q`’, ‘`-Q`’, or ‘`-batch`’ option was used, then Lisp packages should not load any customization files or user profile.

<!---->

*   User Option: **user-mail-address**

    This holds the email address of the user who is using Emacs.

<!---->

*   Function: **user-login-name** *\&optional uid*

    This function returns the name under which the user is logged in. It uses the environment variables `LOGNAME` or `USER` if either is set. Otherwise, the value is based on the effective UID, not the real UID.

    If you specify `uid` (a number), the result is the user name that corresponds to `uid`, or `nil` if there is no such user.

<!---->

*   Function: **user-real-login-name**

    This function returns the user name corresponding to Emacs’s real UID. This ignores the effective UID, and the environment variables `LOGNAME` and `USER`.

<!---->

*   Function: **user-full-name** *\&optional uid*

    This function returns the full name of the logged-in user—or the value of the environment variable `NAME`, if that is set.

    If the Emacs process’s user-id does not correspond to any known user (and provided `NAME` is not set), the result is `"unknown"`.

    If `uid` is non-`nil`, then it should be a number (a user-id) or a string (a login name). Then `user-full-name` returns the full name corresponding to that user-id or login name. If you specify a user-id or login name that isn’t defined, it returns `nil`.

The symbols `user-login-name`, `user-real-login-name` and `user-full-name` are variables as well as functions. The functions return the same values that the variables hold. These variables allow you to fake out Emacs by telling the functions what to return. The variables are also useful for constructing frame titles (see [Frame Titles](Frame-Titles.html)).

*   Function: **user-real-uid**

    This function returns the real UID of the user.

<!---->

*   Function: **user-uid**

    This function returns the effective UID of the user.

<!---->

*   Function: **group-gid**

    This function returns the effective GID of the Emacs process.

<!---->

*   Function: **group-real-gid**

    This function returns the real GID of the Emacs process.

<!---->

*   Function: **system-users**

    This function returns a list of strings, listing the user names on the system. If Emacs cannot retrieve this information, the return value is a list containing just the value of `user-real-login-name`.

<!---->

*   Function: **system-groups**

    This function returns a list of strings, listing the names of user groups on the system. If Emacs cannot retrieve this information, the return value is `nil`.

<!---->

*   Function: **group-name** *gid*

    This function returns the group name that corresponds to the numeric group ID `gid`, or `nil` if there is no such group.

Next: [Time of Day](Time-of-Day.html), Previous: [System Environment](System-Environment.html), Up: [System Interface](System-Interface.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
