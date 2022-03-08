# Interactive and Login Shells

A [shell](dpxbb_unix-shell.md) simply takes a set of commands and runs them, irrespective of where the commands come from, be it a file, user input, etc.

If a user is typing commands into a shell, it is an **interactive** shell. When the shell is only reading commands from a file, it is **non-interactive**. When running a script, a second copy of the shell is executed to run the script; the interactive shell is waiting for the non-interactive shell to finish.

On initial login without a desktop environment, the shell would present the user with the login prompt, hence it being a **login** shell. A desktop environment blurs the lines of shells, as a login shell does not have to be interactive and a shell may be instructed to act as a login shell with options such as `zsh -l` for zsh. Desktop environments often seem to set any directly opened shells as login shells.

The main difference between the shell types is the loading of, and it varies based on the shell itself. For example, the loading of [zsh startup files](jaqct_zsh-startup-files.md) is different than the loading of [bash startup files](8m4jz_bash-startup-files.md).

The current shell may be interrogated to determine if the current instance is a login and/or interactive shell. For example, to determine if the current shell is a login shell with zsh:

```sh
if [[ -o login ]]; then
  print yes
else
  print no
fi
```
