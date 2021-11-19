

Next: [Archive Web Server](Archive-Web-Server.html), Previous: [Multi-file Packages](Multi_002dfile-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]

### 41.4 Creating and Maintaining Package Archives

Via the Package Menu, users may download packages from *package archives*. Such archives are specified by the variable `package-archives`, whose default value contains a single entry: the archive hosted by the GNU project at <https://elpa.gnu.org>. This section describes how to set up and maintain a package archive.

*   User Option: **package-archives**

    The value of this variable is an alist of package archives recognized by the Emacs package manager.

    Each alist element corresponds to one archive, and should have the form `(id . location)`, where `id` is the name of the archive (a string) and `location` is its *base location* (a string).

    If the base location starts with ‘`http:`’ or ‘`https:`’, it is treated as an HTTP(S) URL, and packages are downloaded from this archive via HTTP(S) (as is the case for the default GNU archive).

    Otherwise, the base location should be a directory name. In this case, Emacs retrieves packages from this archive via ordinary file access. Such local archives are mainly useful for testing.

A package archive is simply a directory in which the package files, and associated files, are stored. If you want the archive to be reachable via HTTP, this directory must be accessible to a web server; See [Archive Web Server](Archive-Web-Server.html).

A convenient way to set up and update a package archive is via the `package-x` library. This is included with Emacs, but not loaded by default; type `M-x load-library RET package-x RET` to load it, or add `(require 'package-x)` to your init file. See [Lisp Libraries](https://www.gnu.org/software/emacs/manual/html_node/emacs/Lisp-Libraries.html#Lisp-Libraries) in The GNU Emacs Manual.

After you create an archive, remember that it is not accessible in the Package Menu interface unless it is in `package-archives`.

Maintaining a public package archive entails a degree of responsibility. When Emacs users install packages from your archive, those packages can cause Emacs to run arbitrary code with the permissions of the installing user. (This is true for Emacs code in general, not just for packages.) So you should ensure that your archive is well-maintained and keep the hosting system secure.

One way to increase the security of your packages is to *sign* them using a cryptographic key. If you have generated a private/public gpg key pair, you can use gpg to sign the package like this:

```lisp
gpg -ba -o file.sig file
```

For a single-file package, `file` is the package Lisp file; for a multi-file package, it is the package tar file. You can also sign the archive’s contents file in the same way. Make the `.sig` files available in the same location as the packages. You should also make your public key available for people to download; e.g., by uploading it to a key server such as <https://pgp.mit.edu/>. When people install packages from your archive, they can use your public key to verify the signatures.

A full explanation of these matters is outside the scope of this manual. For more information on cryptographic keys and signing, see [GnuPG](https://www.gnupg.org/documentation/manuals/gnupg/index.html#Top) in The GNU Privacy Guard Manual. Emacs comes with an interface to GNU Privacy Guard, see [EasyPG](https://www.gnu.org/software/emacs/manual/html_node/epa/index.html#Top) in Emacs EasyPG Assistant Manual.

Next: [Archive Web Server](Archive-Web-Server.html), Previous: [Multi-file Packages](Multi_002dfile-Packages.html), Up: [Packaging](Packaging.html)   \[[Contents](index.html#SEC_Contents "Table of contents")]\[[Index](Index.html "Index")]
