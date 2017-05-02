# Set Zsh as the default Task

Open cmder

1. Click on the top left corner to open the menu, then select settings to open the settings window.
2. Go to `Startup` -> `Tasks`. 
3. Create a new Task by selecting the `+` sign, Give it a name such as `zsh`; In the commands window, past the following command

    `set CHERE_INVOKING=1 & %ConEmuDrive%\msys64\usr\bin\bash.exe --login -i -new_console:C:"%ConEmuDrive%\msys64\msys2.ico"`

4. Save Settings
5. Back to `Startup`, In the specified name task section, select the task created above in the list. Save and exit. 
6. Now a new cmder task will default to zsh.

Install `oh-my-zsh`, then add the following to end of `~/.oh-my-zsh/oh-my-zsh.sh`

```sh
export HOME=/c/Users/$USER
export SHELL=/usr/bin/zsh
```