Download and Install msys2 from [MSYS2 Website](http://www.msys2.org/). Follow instructions carefully.

Update MYSYS2 by following the instructions [here](https://github.com/msys2/msys2/wiki/MSYS2-installation)

To Install ZSH as the preferred shell, follow the instructions given in the answer on [stackexchange](https://superuser.com/questions/961699/change-default-shell-on-msys2) which is copied below.

1. From the directory I installed MSYS2, I ran `mingw32_shell.cmd`
2. Upgraded all installed packages by running `pacman -Syu`
3. Installed zsh and curl by running `pacman -Sy zsh curl`
4. Installed git via `pacman -Sy git`
5. Closed the MinGW shell by running `exit` - I did not run `zsh` immediately after installation.
6. Edited `msys2_shell.bat`, `mingw32_shell.bat`, and `mingw64_shell.bat` (or `msys2_shell.cmd`, `mingw32_shell.cmd`, and `mingw64_shell.cmd`) and changed every instance of: `start %WD%mintty -i /msys2.ico /usr/bin/bash --login %*` to: `start %WD%mintty -i /msys2.ico /usr/bin/zsh --login %*` (on line 39 as of 2015-09-23)
7. Ran `mingw32_shell.bat` or `Ran mingw32_shell.cmd`
8. At the zsh configuration menu I select `0` to create the `.zshrc` file.
9. Then I installed oh-my-zsh. Using:

    `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`


### Install CMDER

Install Portable commandline from [here](http://cmder.net/). then put it on your path. 