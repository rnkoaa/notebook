### Add custom Exports to OH-MY-ZSH

To add custom exports to oh-my-zsh, create a file named `exports.zsh` in `$ZSH_DIR\custom`.
Add the custom exports commands into this file and save. Restart the command line and this will be respected. example custom exports which is required on windows for zsh to function properly.

```sh
 export HOME=/c/Users/$USER
 export SHELL=/usr/bin/zsh
```