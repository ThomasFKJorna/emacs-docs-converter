

#### 40.1.2 The Init File

When you start Emacs, it normally attempts to load your *init file*. This is either a file named `.emacs` or `.emacs.el` in your home directory, or a file named `init.el` in a subdirectory named `.emacs.d` in your home directory.

The command-line switches ‘`-q`’, ‘`-Q`’, and ‘`-u`’ control whether and where to find the init file; ‘`-q`’ (and the stronger ‘`-Q`’) says not to load an init file, while ‘`-u user`’ says to load `user`’s init file instead of yours. See [Entering Emacs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Entering-Emacs.html#Entering-Emacs) in The GNU Emacs Manual. If neither option is specified, Emacs uses the `LOGNAME` environment variable, or the `USER` (most systems) or `USERNAME` (MS systems) variable, to find your home directory and thus your init file; this way, even if you have su’d, Emacs still loads your own init file. If those environment variables are absent, though, Emacs uses your user-id to find your home directory.

Emacs also attempts to load a second init file, called the *early init file*, if it exists. This is a file named `early-init.el` in your `~/.emacs.d` directory. The difference between the early init file and the regular init file is that the early init file is loaded much earlier during the startup process, so you can use it to customize some things that are initialized before loading the regular init file. For example, you can customize the process of initializing the package system, by setting variables such as `package-load-list` or `package-enable-at-startup`. See [Package Installation](https://www.gnu.org/software/emacs/manual/html_node/emacs/Package-Installation.html#Package-Installation) in The GNU Emacs Manual.

An Emacs installation may have a *default init file*, which is a Lisp library named `default.el`. Emacs finds this file through the standard search path for libraries (see [How Programs Do Loading](How-Programs-Do-Loading.html)). The Emacs distribution does not come with this file; it is intended for local customizations. If the default init file exists, it is loaded whenever you start Emacs. But your own personal init file, if any, is loaded first; if it sets `inhibit-default-init` to a non-`nil` value, then Emacs does not subsequently load the `default.el` file. In batch mode, or if you specify ‘`-q`’ (or ‘`-Q`’), Emacs loads neither your personal init file nor the default init file.

Another file for site-customization is `site-start.el`. Emacs loads this *before* the user’s init file. You can inhibit the loading of this file with the option ‘`--no-site-file`’.

### User Option: **site-run-file**

This variable specifies the site-customization file to load before the user’s init file. Its normal value is `"site-start"`. The only way you can change it with real effect is to do so before dumping Emacs.

See [Init File Examples](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-Examples.html#Init-Examples) in The GNU Emacs Manual, for examples of how to make various commonly desired customizations in your `.emacs` file.

### User Option: **inhibit-default-init**

If this variable is non-`nil`, it prevents Emacs from loading the default initialization library file. The default value is `nil`.

### Variable: **before-init-hook**

This normal hook is run, once, just before loading all the init files (`site-start.el`, your init file, and `default.el`). (The only way to change it with real effect is before dumping Emacs.)

### Variable: **after-init-hook**

This normal hook is run, once, just after loading all the init files (`site-start.el`, your init file, and `default.el`), before loading the terminal-specific library (if started on a text terminal) and processing the command-line action arguments.

### Variable: **emacs-startup-hook**

This normal hook is run, once, just after handling the command line arguments. In batch mode, Emacs does not run this hook.

### Variable: **window-setup-hook**

This normal hook is very similar to `emacs-startup-hook`. The only difference is that it runs slightly later, after setting of the frame parameters. See [window-setup-hook](Startup-Summary.html).

### Variable: **user-init-file**

This variable holds the absolute file name of the user’s init file. If the actual init file loaded is a compiled file, such as `.emacs.elc`, the value refers to the corresponding source file.

### Variable: **user-emacs-directory**

This variable holds the name of the Emacs default directory. It defaults to `${XDG_CONFIG_HOME-'~/.config'}/emacs/` if that directory exists and `~/.emacs.d/` and `~/.emacs` do not exist, otherwise to `~/.emacs.d/` on all platforms but MS-DOS. Here, `${XDG_CONFIG_HOME-'~/.config'}` stands for the value of the environment variable `XDG_CONFIG_HOME` if that variable is set, and for `~/.config` otherwise. See [How Emacs Finds Your Init File](https://www.gnu.org/software/emacs/manual/html_node/emacs/Find-Init.html#Find-Init) in The GNU Emacs Manual.
