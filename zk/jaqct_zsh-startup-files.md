# Zsh Startup Files

[Zsh](ru1t8_z-shell-zsh-for-short.md) has a simple model of which files to load and when based on the [type of shell](i5k65_interactive-and-login-shells.md).
```
/etc/zshenv
    Always run for every zsh.

~/.zshenv
    Usually run for every zsh unless skipped with NO_RCS.

/etc/zprofile
    Run for login shells.

~/.zprofile
    Run for login shells.

/etc/zshrc
    Run for interactive shells.

~/.zshrc
    Run for interactive shells.

/etc/zlogin
    Run for login shells.

~/.zlogin
    Run for login shells.
```

The 2 login-only files, `.zprofile` and `.zlogin` are only differentiated by when they run and is a historical product of having options to cover users more accustomed to other shells.

Also, 2 files are run at the end: `~/.zlogout` and `/etc/zlogout`, in that order, but there are no standard practices around them and are mostly used to run commands at the end of a session.
